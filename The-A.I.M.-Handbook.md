# The A.I.M. Handbook: Technical Master Schema

This document is the definitive architectural map for the A.I.M. platform. It strictly reflects the actual underlying codebase, defining the modular components of the system and the protocols that ensure continuity and sovereignty.

---

## SECTION 1: THE ROOT ARCHITECTURE

### 1.1 `AGENTS.md` (The Index & Soul)
- **Role:** Lean Orchestrator & Cognitive Baseline.
- **Function:** It is an explicit **Table of Contents** injected into the agent's core context. Instead of holding massive walls of text, it directs the agent to query the [Engram DB](Layered-Engram-Architecture) for specific technical policies using `aim search` or read the synthesized lore in `wiki/index.md`.
- **Cognitive Guardrails:** It permanently encodes the Operator's chosen grammar level, execution mode (Autonomous vs Cautious), and the GitOps workflow mandate.

### 1.2 Initialization & Provisioning (`aim init`)
- **Function:** A dynamic, decoupled scaffolding wizard.
- **Clean Sweep:** Allows the user to wipe A.I.M. internal Project Docs (to prevent identity crisis on new projects) and optionally wipe the raw brain sync data (`archive/sync/`).
- **Zero-RAG Mode:** Supports an `aim init --light` flag that completely disables the LanceDB Engram DB and heavy RAG pipelines, relying strictly on the Continuity files for lightweight operations.

### 1.3 The Command Center (`aim config` / `aim tui`)
- **Function:** The interactive Terminal UI.
- **Role:** The Operator can hot-swap the AI's personality, adjust interval settings, switch LLM models across different tiers (e.g., configuring the Subconscious Wiki Daemon), and define the machine's Cognitive Architecture (Monolithic, Frontline Agent, or Subconscious Node).

---

## SECTION 2: THE FEDERATED BRAIN (THE ARCHIPELAGO MODEL)
To eliminate the scaling bottleneck of monolithic databases, A.I.M. utilizes a Native Parquet ROM vs RAM architecture. It uses a **[Hybrid RAG](Feature-Hybrid-RAG)** engine, blending dense Vector Embeddings with Tantivy FTS Lexical matching.

### 2.1 The Databases
- **`memory_lance` (The RAM):** The live, hyper-local context of your active repository (flight recorders and embedded wiki lore).
- **`archive/cartridges/*.parquet` (The ROM):** Immutable, highly compressed Parquet cartridges providing cross-project skills and read-only documentation (e.g., the entire Django framework).

### 2.2 The Knowledge Map (`aim map` & `aim search`)
- **Function:** Agents use `aim map` to see a lightweight index of available knowledge across all databases, and `aim search "query"` to retrieve specific chunks via Hybrid RAG.

### 2.3 Foundry Ingestion & Cartridge Baking (`aim bake`)
- **Function:** The `foundry/` folder is a dedicated intake zone for technical references. You can manufacture atomic `.parquet` cartridges directly from the documentation using `aim bake`.

---

## SECTION 3: THE PERSISTENT LLM WIKI (DUAL-SEARCH ARCHITECTURE)
The monolithic `MEMORY.md` file and legacy cascading memory tiers have been deprecated. Memory is now an auto-maintaining Knowledge Base that lives in the `wiki/` directory.

### 3.1 The Dual-Search Architecture
- **Obsidian Native Sync:** The `wiki/` directory is purely native Markdown. You can open it directly as an Obsidian Vault. Changes made by the Subconscious Daemon instantly appear in your Knowledge Graph.
- **Fast Lexical Search (`aim wiki search`):** Builds an *in-memory* Tantivy index on the fly for 0ms latency exact-keyword searches of the markdown wiki, protecting the agent's token wallet.
- **Deep Semantic Search (`aim search`):** The synthesized `wiki/*.md` files are also embedded into the `memory_lance` vector store so the Conscious Agent can retrieve architectural lore semantically.

### 3.2 The Reincarnation Hook
Memory is compiled and distilled securely in a continuous loop when `/reincarnate` is triggered:
1. **Flight Recorder:** The dying agent generates a clean Markdown log of the session.
2. **Vector Ingestion:** That log is mathematically embedded into the native LanceDB `memory_lance` RAM pool for instant Hybrid RAG search retrieval.
3. **The Drop Zone:** The core takeaways (Signal Skeleton) are extracted and dropped into `wiki/_ingest/`.
4. **Subconscious Synthesis:** The Subconscious Wiki Daemon wakes up, reads the ingest folder, seamlessly updates the `wiki/` markdown files, logs the action to `wiki/log.md`, and re-embeds the updated wiki into `memory_lance`.

---

## SECTION 4: ETERNAL RECALL (HISTORY & CONTINUITY)

### 4.1 Event-Driven Archiving (`aim_core/handoff_pulse_generator.py`)
- **Role:** Event-Driven Persistence.
- **Function:** Triggered mechanically during handoffs. It scrubs raw JSON noise and converts sessions to clean Markdown (`archive/history/`), routing them instantly into the vector ingestion pipeline without unnecessary monolithic LLM calls.

### 4.2 The Reincarnation Protocol (`aim reincarnate`)
- **Function:** Before the context window fills entirely, A.I.M. teleports its cognitive state to a fresh terminal session.
- **Directive:** Passes "Will" and a strict Gameplan via `continuity/REINCARNATION_GAMEPLAN.md` and `continuity/CURRENT_PULSE.md`, preventing agent hallucination at high token counts.

### 4.3 Crash Recovery (`aim crash`)
- **Function:** If an agent suffers a catastrophic context overflow (e.g., V8 memory heap crash), this protocol autonomously extracts the session signal from the dead container, generates a clean handoff bridge, and syncs open issues without losing progress.

---

## SECTION 5: SAFETY & SOVEREIGNTY

### 5.1 Atomic Deployments (The GitOps Bridge)
- **Report:** Create tickets autonomously using `aim bug`.
- **Isolate:** Branch out surgically using `aim fix <id>`.
- **Release:** Merge and release using `aim push`.

---

## SECTION 6: SYSTEM MAINTENANCE

### 6.1 The Decoupled Sovereign Update (`aim update engine` / `aim update project`)
- **Role:** High-Fidelity Sync.
- **Function:** Safely pulls the latest A.I.M. OS updates (`aim update engine`) or your local target repository (`aim update project`) without wiping your local memory database.

---

"I believe I've made my point." — **A.I.M.**