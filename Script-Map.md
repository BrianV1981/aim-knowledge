# The Codebase Map (A.I.M. v1.56.0)

This document is the literal map of every internal script running the A.I.M. OS. Use it to understand where business logic lives.

---

## 1. Core Source (`src/`)
*The pure Python engines driving the backend architecture.*

*   **`bootstrap_brain.py`**: The initialization engine. Re-indexes `GEMINI.md`, `wiki/`, and `core/` into the `project_core.db`.
*   **`config_utils.py`**: The single source of truth for loading, validating, and auto-repairing `core/CONFIG.json` paths.
*   **`daemon.py`**: The overarching background service orchestrator.
*   **`handoff_pulse_generator.py`**: The engine that distills the current terminal JSON into `CURRENT_PULSE.md` and mechanically generates the `LAST_SESSION_FLIGHT_RECORDER.md`. It triggers the Subconscious Wiki Daemon and vector ingestion.
*   **`heartbeat.py`**: The zero-token diagnostic tool. Verifies the integrity of the Federated DBs, hooks, and sync folders.
*   **`maintenance.py`**: The automated janitor. Purges old logs, cleans the workspace, and manages retention policies.
*   **`mcp_server.py`**: The Model Context Protocol server, enabling external IDEs (Cursor/VS Code) to query the Federated DBs and read memories.
*   **`reasoning_utils.py`**: The universal LLM client. Dynamically routes API calls based on the Tiers defined in `CONFIG.json` (Google vs Anthropic vs Local).
*   **`retriever.py`**: The Hybrid RAG search engine. Handles vector embeddings (Nomic) and lexical search (FTS5) across the Archipelago databases (`project_core.db`, etc.).
*   **`sovereign_sync.py`**: The outbound transport layer. Converts the SQLite tables into human-readable `.jsonl` files in `archive/sync/`.
*   **`back-populator.py`**: The inbound transport layer. Rebuilds a corrupted or empty SQLite database natively from the `.jsonl` backup files.
*   **`wiki_tools.py`**: The Persistent LLM Wiki engine. Handles the fast lexical `aim wiki search` and the background `aim wiki process` Subconscious Daemon logic.

### 1.1 DataJack Plugins (`src/plugins/datajack/`)
*   **`aim_bake.py`**: Compiles a directory of text/markdown files into a portable `.engram` ZIP cartridge.
*   **`aim_exchange.py`**: Handles the ingestion and decryption of `.engram` files back into the local databases.
*   **`forensic_utils.py`**: The SQLite database wrapper. Manages all `INSERT` and `SELECT` queries for the Federated DBs.
*   **`quarantine_daemon.py`**: The background watchdog that intercepts, validates, and scans incoming Swarm cartridges for adversarial prompt injections.

---

## 2. CLI Router & Scripts (`scripts/`)
*The front-end commands accessible via the `aim` global alias.*

*   **`aim_cli.py`**: The central router. Parses `aim search`, `aim wiki`, `aim update engine` / `aim update project`, etc., and dispatches the corresponding script.
*   **`aim_config.py`**: The Interactive TUI (Cockpit). Modifies `CONFIG.json` using the `questionary` library.
*   **`aim_crash.py`**: The recovery script. Safely extracts the signal from a corrupted V8 heap crash without losing context.
*   **`aim_delegate.py`**: The sub-agent spin-up protocol for distributed background processing.
*   **`aim_init.py`**: The setup wizard. Executed upon first repository installation to build the databases, wiki, and hooks.
*   **`aim_reincarnate.py`**: The Context Pruning teleport script. Asks for a Gameplan, spawns a new Tmux agent, and assassinates the bloated one.
*   **`aim_router.py`**: Handles dynamic routing algorithms.
*   **`aim_torrent.py`**: The P2P [DataJack](The-DataJack-Protocol) Swarm daemon for sharing cartridges.
*   **`aim_vault.py`**: The encrypted local key-store for holding API keys securely without putting them in plaintext configs.
*   **`aim_push.sh`**: The GitOps validation wrapper. Prevents direct pushes to `main` and enforces the branch/issue protocol.
*   **`aim_batch_merge.py`**: Utility for merging multiple pull requests simultaneously.
*   **`extract_signal.py`**: The Zero-Token Scribe. Uses pure Python logic to strip 85% of JSON noise from native CLI transcripts.
*   **`obsidian_sync.py`**: The Outbound [Obsidian Bridge](Obsidian-Bridge-Architecture). Mirrors A.I.M.'s memory files into a local Obsidian Vault.
*   **`obsidian_pull.py`**: The Inbound [Obsidian Bridge](Obsidian-Bridge-Architecture) (`aim ingest`). Pulls manual edits from the vault back into the workspace.
*   **`sync_issue_tracker.py`**: The local synchronization engine for `ISSUE_TRACKER.md`.

---

## 3. Benchmarks (`scripts/benchmarks/`)
*Automated, repeatable testing harnesses for the platform.*

*   **`setup_environments.sh`**: Creates the `django_control` and `django_matrix` boilerplate folders for A/B testing.
*   **`calculate_economics.py`**: Parses the resulting JSON transcripts to calculate exact token usage and API costs.
*   **`recover_json_logs.py`**: Extracts the `.gemini/tmp/` logs from the benchmark folders so they can be committed to the repo for proof.