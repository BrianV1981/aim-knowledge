> **⚠️ DEPRECATION NOTICE:** This page details the legacy SQLite-based RAG 4.0/4.2 architecture. A.I.M. has since been upgraded to **RAG 5.2 (The ROM vs RAM Architecture)**, which natively utilizes LanceDB and Apache Arrow Parquet cartridges. This page is preserved for historical context regarding the evolution of the memory system.

# A.I.M. RAG 4.0 Architecture (The Surround Method)

**Status:** Live
**Core Components:** `nomic-embed-text`, `qwen3.5:4b`, `llava`, SQLite FTS5
**Hardware Assumption:** WSL2 (Ubuntu) bridged to Windows Host GPU (AMD RX 9070 XT)

---

## 1. The Core Dilemma: Semantic Density vs. LLM Context
In conversational RAG pipelines, there is a fundamental conflict between the Vector Database and the LLM:
*   **Vector Databases** need tiny, highly dense chunks (e.g., 1-turn) to generate high-scoring, accurate keyword matches.
*   **LLMs** need massive, multi-turn chunks to resolve coreferences (e.g., knowing who "she" is or what "that code" refers to from 3 turns ago).

RAG 4.0 perfectly decouples these two conflicting needs using the **Small-to-Big (Surround) Method**.

---

## 2. Ingestion Phase: Strict 1-Turn Chunking
During ingestion (`forensic_utils.py -> ingest_document`), the system heavily cleans the historical flight recorders before vectorizing them.

1.  **Monologue Scrubber:** The ingestion script violently deletes all `> **Internal Monologue:**` blocks. This prevents the agent's meta-reasoning ("I am currently investigating...") from diluting the semantic density of the actual facts and actions.
2.  **Speaker Boundaries:** The remaining text is chopped strictly by speaker (`## 👤 USER` and `## 🤖 A.I.M.`). Every single chunk is exactly 1 turn.
3.  **Vectorization:** These dense 1-turn chunks are passed to `nomic-embed-text` (running on the Windows host GPU) to generate the high-dimensional float array for the database.

---

## 3. The Parent-Child Anomaly (Massive Chunk Summarization)
Occasionally, a single turn exceeds 2,000 characters (e.g., a massive codebase paste or a giant tool output). If we embed 100,000 characters into a single `nomic` vector, the semantic signal is destroyed by noise.

**The Fix:**
When `ingest_document` detects a chunk >2,000 characters, it triggers the local Sovereign LLM (default: `qwen3.5:4b`) to generate a "Semantic Anchor".
*   The raw massive text is truncated to a safe context limit (e.g., 100k characters) to prevent KV cache generation timeouts.
*   `qwen3.5:4b` natively reads the block and outputs a dense, factual bulleted list.
*   **The Child:** The Qwen summary is embedded by `nomic` and inserted into the database.
*   **The Parent:** The full 100,000-character raw text is inserted into the database *without* an embedding, linked via `parent_id`. 
*   **The Disk Cache:** To prevent crippling API timeouts and wasted GPU compute on re-ingestion, every Qwen summary is instantly permanently saved to `archive/massive_turn_cache.json`.

---

## 4. Multimodal Grounding (LLaVA)
When the ingestion script encounters a markdown image `![alt](url)`, it intercepts the URL before chunking.
*   It passes the image to the local `llava` vision model.
*   LLaVA flattens the visual data into a highly descriptive text string.
*   The script replaces the image link inline with `*[Visual Description: ...]*`, allowing the vector database to "see" the image semantically.
*   These descriptions are permanently cached in `archive/llava_universal_cache.json`.

---

## 5. Retrieval Phase: The `_expand_and_deduplicate` Protocol
When an agent or user runs a search (Hybrid FTS5 + Nomic), the database finds the absolute best 1-turn chunk (The "Small" hit). 

However, before returning the result to the LLM, `forensic_utils.py` executes the "Surround" logic:
1.  **Expansion:** It queries SQLite for the `+/- 2` turns immediately surrounding the winning chunk, reconstructing the conversational timeline.
2.  **De-duplication:** If Search Hit #1 and Search Hit #2 are from the same conversation, their surrounding turns will overlap. The script merges them into a single chronological block to save context window tokens.
3.  **Parent Protection:** If one of the surrounding turns happens to be a massive "Parent" block (e.g., a 100,000-character code dump), the script dynamically truncates it to `... [MASSIVE TEXT BLOCK OMITTED TO PRESERVE CONTEXT LIMIT] ...` to guarantee the LLM doesn't OOM. It only returns the full Parent text if the Parent itself was the explicit search target.

---

## 6. Critical Infrastructure: The WSL2 Networking Bridge
To successfully offload the heavy `nomic`, `qwen3.5:4b`, and `llava` models to a Windows host GPU (e.g., AMD RX 9070 XT) from inside WSL2 Linux:

1.  **Disable Linux Daemon:** The CPU-bound Linux `ollama.service` must be permanently disabled (`systemctl stop ollama && systemctl disable ollama`) to prevent VRAM contention and 5-minute CPU timeouts.
2.  **Windows Host:** The Windows Ollama app must be running with the Environment Variable `OLLAMA_HOST=0.0.0.0` to accept connections from the WSL Subsystem.
3.  **Python IPv6 Bug:** Python's `requests` library will hang on a 500 error if instructed to hit `http://localhost:11434` because it resolves `localhost` to the dead IPv6 `::1` tunnel. **You must explicitly hardcode `http://127.0.0.1:11434` in all Python payloads** to force the IPv4 bridge.