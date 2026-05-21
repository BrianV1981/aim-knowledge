# A.I.M. TUI Architecture Map

The **Sovereign Cockpit (TUI)** is the central control plane for the A.I.M. OS. It allows the operator to configure the [cognitive routing](Feature-Cognitive-Routing), behavioral guardrails, and memory retention policies without manually editing JSON files.

The TUI is entirely executed via `scripts/aim_config.py`. Below is the technical mapping of every feature to its associated configuration file and system logic.

---

## 1. Run Cognitive Health Check (Test All)
- **Logic:** Iterates through the active cognitive tiers (Primary Brain and Subconscious Wiki Daemon) defined in `core/CONFIG.json`.
- **Execution:** Calls `generate_reasoning("Respond with 'OK'")` in `src/reasoning_utils.py` for each active tier.
- **Associated Files:** `core/CONFIG.json`, `src/reasoning_utils.py`, `scripts/aim_config.py`.

## 2. Manage Secret Vault (API Keys)
- **Logic:** Interfaces directly with the underlying Linux/macOS keyring to securely store, retrieve, or delete API credentials. 
- **Execution:** Uses the Python `keyring` library (wrapped in `scripts/aim_vault.py`). 
- **Associated Files:** `scripts/aim_vault.py`.

## 3. Configure Default Brain
- **Logic:** Configures the `provider`, `model`, `endpoint`, and `auth_type` for `default_reasoning`.
- **Role:** The Primary Brain (Conscious Agent) used for interactive coding, reasoning, and terminal execution.
- **Associated Files:** `core/CONFIG.json`, `src/reasoning_utils.py`.

## 4. Configure Subconscious Wiki Daemon
- **Logic:** Configures the `provider`, `model`, `endpoint`, and `auth_type` for `tier1`.
- **Role:** The Subconscious LLM that runs in the background (`hooks/session_summarizer.py`) to synthesize the `wiki/_ingest/` folder into the Persistent LLM Wiki. Can be set to a free local model to save tokens.
- **Associated Files:** `core/CONFIG.json`, `src/reasoning_utils.py`.

## 5. Manage MCP Server (IDE Integration)
- **Logic:** Manages the FastMCP server for IDE integration (Cursor/VS Code).
- **Execution:** Uses `subprocess` to launch `src/mcp_server.py`.
- **Associated Files:** `src/mcp_server.py`, `skills/*`.

## 6. Update Operator Profile & Behavior
- **Logic:** Asks the user for both operator identity fields and behavioral guardrails.
- **Execution:** Rewrites `core/OPERATOR.md`, `core/OPERATOR_PROFILE.md`, and updates `AGENTS.md`.
- **Associated Files:** `AGENTS.md`, `core/OPERATOR.md`, `core/OPERATOR_PROFILE.md`.

## 7. Update Synced Knowledge Vault Path
- **Logic:** Sets the absolute path to a local Synced Knowledge Vault (Syncthing/Obsidian) for [the Decoupled Brain](The-Decoupled-Brain) bridge.
- **Execution:** Writes the path to `["settings"]["obsidian_vault_path"]` in `core/CONFIG.json`. 

## 8. Configure Cognitive Architecture
- **Logic:** Toggles the operational mode of the background daemon (monolithic, frontline, or subconscious).
- **Execution:** Writes the mode to `["settings"]["cognitive_mode"]` in `core/CONFIG.json`.

## 9. Archive Retention
- **Logic:** Configures how many days the system should keep raw JSON transcripts and old proposals.
- **Execution:** Writes an integer to `["settings"]["archive_retention_days"]`.

## 10. Set Agent Persona (Specialty Mandate)
- **Logic:** Injects a specialized mandate (e.g., Frontend Architect) into the top of the AI's system prompt.
- **Associated Files:** `AGENTS.md`.

## 11. Configure Cognitive Mantra (Anti-Drift)
- **Logic:** Configures the `cognitive_mantra.py` watchdog timer to prevent behavioral drift during long autonomous runs.
- **Associated Files:** `core/CONFIG.json`, `hooks/cognitive_mantra.py`.

## 12. Configure LAST_SESSION_FLIGHT_RECORDER.md
- **Logic:** Determines the maximum number of lines the Continuity Engine preserves during handoff.
- **Special Case:** Setting to `0` enables **Full Session History** (no truncation).
- **Associated Files:** `core/CONFIG.json`, `src/handoff_pulse_generator.py`.

## 13. Reincarnation Protocol
- **Logic:** Toggles the `auto_rebirth` feature.
- **Execution:** If enabled, A.I.M. will automatically spawn a fresh terminal session when the context limit is breached.
- **Associated Files:** `core/CONFIG.json`, `scripts/aim_reincarnate.py`.

## 14. BitTorrent Swarm Integration
- **Logic:** Opts the agent into the decentralized P2P knowledge network.
- **Execution:** Configures `swarm_enabled`, `max_download_speed`, `seeding_ratio`, and `rpc_port` inside `core/CONFIG.json` for the `aria2p` daemon.
- **Associated Files:** `core/CONFIG.json`, `scripts/aim_torrent.py`.