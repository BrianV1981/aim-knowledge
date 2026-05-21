# [DEPRECATED] The Cascading Memory Engine

> ⚠️ **ARCHITECTURAL UPDATE:** The 5-Tier Cascading Memory Engine described in this document has been officially deprecated and ripped out of the A.I.M. codebase. 
> 
> After rigorous testing, we realized that forcing memories through 5 chronological tiers (Hourly -> Daily -> Weekly) created unnecessary latency, leading to a loss of "Epistemic Certainty." 
>
> A.I.M. initially transitioned to a "Single-Shot Sovereign Memory Pipeline" centered around a single `MEMORY.md` file, but this too has now been deprecated. We have officially transitioned to the **[Persistent LLM Wiki Architecture](The-Persistent-LLM-Wiki-Architecture)**.
>
> Memory is now handled via a **Dual-Search Architecture**: Raw session flight recorders are mathematically embedded into the SQLite `project_core.db` for instant Hybrid RAG retrieval, while a Subconscious Daemon seamlessly synthesizes architectural decisions into a multi-file, Obsidian-synced `wiki/` directory. 
> 
> *This page is preserved purely for historical context regarding the initial design philosophy.*

---

*(The rest of the legacy content is preserved below)*

## 1. Overview
The **Cascading Memory Engine** is the digestive system of A.I.M. It takes the "Raw mind" (JSON terminal logs) and processes it through multiple filters until it becomes "Durable Truth" (the `MEMORY.md` file).

## 2. The 5-Stage Waterfall
The refinement process is governed by time-based triggers. Each stage only processes the "unrefined" output of the stage below it.

1.  **Tier 1 (Harvester):** Runs every **1 hour**. Extracts the "Signal Skeleton" and produces reasoning-backed ARC reports (Adds/Removes/Contradicts).
2.  **Tier 2 (Proposer):** Runs every **12 hours**. Consolidates the hourly reports into a structured Delta.
3.  **Tier 3 (Refiner):** Runs every **24 hours**. Synthesizes multiple proposals into a single Daily State.
4.  **Tier 4 (Consolidator):** Runs every **72 hours (3 Days)**. Distills the daily states into macro architectural milestones.
5.  **Tier 5 (Archivist):** Runs every **144 hours (6 Days)**. Converts the weekly milestones into dense, factual axioms and **automatically applies them** to the durable memory file.

## 3. The "Consume & Clean" Protocol
To prevent redundancy and token burn, each stage "consumes" its inputs.
*   Once Tier 2 has successfully refined the Tier 1 reports, it immediately **Archives** or **Deletes** those reports.
*   This ensures that every refinement turn is only looking at **new data**.

## 4. Manual Override (`aim commit`)
The operator does not have to wait for the waterfall. You can run `aim commit` at any time to instantly trigger an AI-driven merge of the latest refinement proposal into your durable memory.

## 5. Configuration
All intervals and the cleanup mode (Archive vs Delete) are live-configurable via:
```bash
aim tui  # Option 13: Configure Waterfall Pipeline
```

---
*For the complete technical breakdown, see: [[[Waterfall Memory Architecture](Waterfall-Memory-Architecture)]]*
