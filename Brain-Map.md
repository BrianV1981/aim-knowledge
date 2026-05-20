# A.I.M. Brain Map (Cognitive Architecture)

This document maps the complete anatomical structure of the A.I.M. "Brain." 

## 1. The Failsafe Snapshot Layer
*   **Trigger:** Executed silently in the background on every turn by `hooks/failsafe_context_snapshot.py`.
*   **Function:** Saves a rolling JSON backup to `continuity/INTERIM_BACKUP.json` and a markdown tail to `continuity/FALLBACK_TAIL.md`.
*   **Purpose:** Session rescue during crashes.

## 2. The Continuity Pulse (The Handoff)
*   **Trigger:** Executed by `aim reincarnate`.
*   **Function:** Generates `CURRENT_PULSE.md` (the State) and writes the executive directive to `continuity/REINCARNATION_GAMEPLAN.md`.
*   **Purpose:** To teleport context to a fresh agent before "System Prompt Fade" occurs.

## 3. The Persistent LLM Wiki (Synthesized Lore)
*   **Trigger:** The Subconscious Daemon runs automatically after `/reincarnate` or via `aim wiki process`.
*   **Storage:** `wiki/` directory (Native Markdown).
*   **Mechanism:** Drops "Signal Skeletons" into `wiki/_ingest/` and uses a background LLM to weave the new knowledge into the existing Markdown wiki files.
*   **Purpose:** Human-readable, auto-maintaining architectural memory.

## 4. The Native Parquet Engine (The Subconscious)
*   **Trigger:** Automatic vector ingestion during reincarnation and `aim ingest`.
*   **Storage:** LanceDB RAM and Parquet ROM cartridges (`memory_lance`, `archive/cartridges/*.parquet`).
*   **Function:** [Hybrid RAG](Feature-Hybrid-RAG) (Semantic Vectors + Tantivy FTS).
*   **Purpose:** Token-efficient semantic retrieval for the Conscious Agent via `aim search`.

## 5. Eternal Recall (History Search)
*   **Trigger:** Event-driven handoff pulse generator (`aim_core/handoff_pulse_generator.py`).
*   **Storage:** `archive/history/` (Markdown) and embedded into `memory_lance`.
*   **Function:** Dedicated keyword search across the entire project history.

## 6. Sovereign Synchronization (The Export Layer)
*   **Trigger:** `aim_core/sovereign_sync.py` running during `aim push`.
*   **Function:** Translates the memory into deterministic `.jsonl` files in `archive/sync/`.
*   **Purpose:** Git-friendly, mergeable brain backups.