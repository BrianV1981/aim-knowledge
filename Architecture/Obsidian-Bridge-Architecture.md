# Architecture: The Obsidian Bridge (Sovereign Mirror)

## 1. Intent
A.I.M. utilizes a local Obsidian vault as a secondary "Offloaded Brain." This provides a graphical interface for human operators to observe, explore, and manually intervene in A.I.M.'s cognitive state using familiar Markdown paradigms and wiki-links.

The Obsidian Bridge provides **[Epistemic Certainty](Benchmark-Epistemic-Certainty)** by enabling two-way synchronization:
- **Push (Outbound):** A.I.M. automatically mirrors its raw logs to an external vault, while the `wiki/` directory acts as a native Vault itself.
- **Pull (Inbound):** Human operators can edit documents in Obsidian, and A.I.M. will ingest those changes back into its operational workspace and [Engram DB](Layered-Engram-Architecture) vector stores.

## 2. Synchronization Doctrine

### 2.1 Native Sync (The Persistent LLM Wiki)
The `wiki/` directory in the A.I.M. workspace is entirely native Markdown. 
- **The Protocol:** You simply open the `wiki/` directory directly as your Obsidian Vault. 
- **The Result:** Whenever the Subconscious Daemon writes synthesized architectural lore to the wiki files, your Obsidian Knowledge Graph updates in real-time. Zero API calls or transport scripts required.

### 2.2 Manual Pull (Obsidian -> A.I.M. Vector Store)
The inbound synchronization is explicitly triggered by the operator when they wish to commit their manual Obsidian edits to A.I.M.'s internal vector search capabilities.
- **Trigger:** The operator executes `aim ingest` (powered by the `obsidian_pull.py` transport layer).
- **Re-indexing:** To ensure the Conscious Agent can "feel" the manual edits during semantic RAG searches (`aim search`), `obsidian_pull.py` acts as a transport script that programmatically triggers A.I.M.'s existing indexing engine (`aim index` / `bootstrap_brain.py`). This guarantees the FTS5/Vector databases immediately reflect the operator's manual wiki edits.

## 3. Operator Intervention Guidelines

1. **Observation First:** Treat the Obsidian vault primarily as a dashboard. Use it to read continuity pulses and review the synthesized wiki lore without interrupting A.I.M.'s current terminal session.
2. **Surgical Edits:** When manual correction is required (e.g., fixing a hallucination in `wiki/index.md` or clarifying a `docs/POLICY_*.md` document), edit the file directly in Obsidian.
3. **Commit the Ingest:** Immediately after making a manual edit in Obsidian, run `aim ingest` in your terminal to synchronize the workspace and re-index the Federated databases.
4. **Heavy Lifting:** For large-scale architectural changes, continue using A.I.M.'s CLI and delegation tools. The Obsidian bridge is best utilized for high-precision, human-in-the-loop state corrections and documentation refinement.