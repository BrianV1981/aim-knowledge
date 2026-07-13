# A.I.M. Knowledge Vault

Welcome to the A.I.M. Knowledge Vault. This repository serves as a public Obsidian Vault and deep-lore archive for the [A.I.M. Exoskeleton](https://github.com/BrianV1981/aim) project. 

Because the main `aim` repository and its live wiki are kept ruthlessly lean (containing only the operational User Manual), this repository acts as the designated "dumping ground" and structured archive for all the heavy philosophical essays, architectural blueprints, and benchmark forensics that dictate how A.I.M. was built.

## 📂 Directory Structure

To maintain a "rhyme and reason" across this massive lore base, the markdown files have been categorized into the following directories:

### `Architecture/`
Contains the definitive schemas and historical blueprints of the OS.
*   **The Master Schema / Handbook:** The literal data flow models.
*   **RAG Upgrades:** The evolution from SQLite (RAG 4.0) to LanceDB Parquet (RAG 5.21).
*   **Feature Explanations:** Deep dives into Hybrid RAG, Cognitive Routing, and GitOps bridges.

### `Benchmarks/`
Contains the empirical data and forensic reports proving the A.I.M. architecture.
*   **LongMemEval Victory:** Documentation of the 95.6% recall score and 18GB compaction.
*   **Django Matrix:** A 50% context reduction proven against raw Gemini baseline.
*   **Vibe Coding vs TDD:** Proof that unconstrained agents generate fatal data races.
*   **Aerospace & Render:** Sandbox evaluations of autonomous debugging.

### `Philosophy_and_Concepts/`
The essays and manifestos defining the "Why" behind A.I.M.
*   **A.I.M. vs The World:** A direct critique of Autonomous Agent and Virtual OS frameworks.
*   **The 30% Rule:** Why context windows collapse and why reincarnation is mandatory.
*   **The MMO Botter's Advantage:** Why strict rules beat raw model scaling.
*   **The Eureka Protocol & Cartridge Farming:** How agents generate "sweat equity" into exportable skills.
*   **Grok Review:** A third-party AI audit of the entire architecture.

### `Dataset_Forensics/`
Detailed logs repairing the broken upstream LoCoMo V2 dataset.
*   **V2 Purification & Rebuilds:** Resolving link rot and Ground Truth hallucinations.
*   **Master Tagged Review Log:** The 1,175-line log of every corrected QA pair.

### `Infrastructure/`
ADRs (Architecture Decision Records) and sentinel setup guides.
*   **The Ghost Orchestrator:** How A.I.M. uses `tmux` to bypass the API Wall and silent 429 timeouts.
*   **Heartbeat & Pinger:** Detached watchdogs ensuring system compliance.
*   **Wiki Overhaul:** The strategy documents that led to the creation of this very repository.

### `Guides_and_Manuals/`
Historical copies of user guides, TUI maps, and prompt ledgers. (For the most up-to-date quickstart instructions, please refer to the main [A.I.M. Live Wiki](https://github.com/BrianV1981/aim/wiki)).

<!-- AIM_ECOSYSTEM_START -->
### 🧬 The A.I.M. Ecosystem

Modular A.I.M. (Actual Intelligent Memory) repositories. **Flagship engine: [aim-agy](https://github.com/BrianV1981/aim-agy).**

**Active vessels (CLI hosts):**
- **[aim-agy](https://github.com/BrianV1981/aim-agy)** — Core engine (Antigravity / post–Gemini-CLI line). *Flagship.*
- **[aim-grok](https://github.com/BrianV1981/aim-grok)** — Grok CLI vessel (hybrid memory, GitOps, wiki).
- **[aim-opencode](https://github.com/BrianV1981/aim-opencode)** — OpenCode CLI vessel.
- **[aim-codex](https://github.com/BrianV1981/aim-codex)** — Codex-native vessel (**on the horizon** — not deprecated).

**Tools & workspaces:**
- **[aim-connect](https://github.com/BrianV1981/aim-connect)** — Self-hosted remote workspace web UI.
- **[aim-tmux-dashboard](https://github.com/BrianV1981/aim-tmux-dashboard)** — Terminal multi-session monitor.
- **[aim-browser](https://github.com/BrianV1981/aim-browser)** — Headed Chromium CDP engine + browser **skill suite**.
- **[aim-google](https://github.com/BrianV1981/aim-google)** — Google Workspace CLI (Gmail, Drive, Calendar, …).
- **[aim-flight-recorder](https://github.com/BrianV1981/aim-flight-recorder)** — Forensic Markdown session extractor.
- **[aim-boardroom](https://github.com/BrianV1981/aim-boardroom)** — Multi-agent orchestration room (OS multiplexing + artifacts).
- **[aim-skills](https://github.com/BrianV1981/aim-skills)** — **Skills index / multi-CLI install registry** (agy, grok, opencode, codex).

**DNA, comms & lore:**
- **[aim-coagents](https://github.com/BrianV1981/aim-coagents)** — DNA bank for sovereign co-agent blueprints.
- **[aim-knowledge](https://github.com/BrianV1981/aim-knowledge)** — Public Obsidian vault / deep-lore archive.
- **[aim-chalkboard](https://github.com/BrianV1981/aim-chalkboard)** — Optional cross-host async git mailbox (PoC; default same-host comms = **aim-communicate** skill).

**Deprecated / not maintained:**
- **[aim](https://github.com/BrianV1981/aim)** — Original **Gemini CLI** framework. Deprecated after loss of practical subscription access; **Great Migration → aim-agy**.
- **[aim-swarm](https://github.com/BrianV1981/aim-swarm)** — Legacy Python swarm factory → use **aim-coagents** + aim-agy spawn.
- **aim-claude / Anthropic-line vessels** — **Done.** Operator does not develop against Anthropic. Use aim-agy / aim-grok / aim-opencode (or aim-codex when ready).

Full map: see **aim-skills** `docs/AIM_ECOSYSTEM_MAP.md` or Operator artifact `AIM_ECOSYSTEM_MAP.md`.
<!-- AIM_ECOSYSTEM_END -->

