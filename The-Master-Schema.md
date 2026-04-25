# A.I.M. Technical Handbook (Master Schema)

This document is the definitive architectural map for the A.I.M. platform. It strictly reflects the actual underlying codebase, defining the modular components of the system and the protocols that ensure continuity and sovereignty.

---

## SECTION 1: THE ROOT ARCHITECTURE

### 1.1 `GEMINI.md` (The Index & Soul)
- **Role:** Lean Orchestrator & Cognitive Baseline.
- **Function:** It is an explicit **Table of Contents** injected into the agent's core context. Instead of holding massive walls of text, it directs the agent to query the [Engram DB](Layered-Engram-Architecture) for specific technical policies using `aim search` or read the synthesized lore in `wiki/index.md`.
- **Cognitive Guardrails:** It permanently encodes the Operator's chosen grammar level, execution mode (Autonomous vs Cautious), and the GitOps workflow mandate.

### 1.2 The Initialization Overhaul (`aim init`)
- **Function:** A dynamic, decoupled scaffolding wizard.
- **Clean Sweep:** Allows the user to independently wipe Project Docs (Roadmap, Changelog) and/or the AI Brain ([Engram DB](Layered-Engram-Architecture)) when repurposing the A.I.M. template for a new codebase.
- **Zero-RAG Mode:** Supports an `aim init --light` flag that completely disables the SQLite Engram DB and heavy RAG pipelines, relying strictly on the Continuity files for lightweight operations.
- **The TUI Updater:** If behavioral questions are skipped during installation, the Operator can hot-swap the AI's personality and rules dynamically using the `aim tui` cockpit.

---

## SECTION 2: THE FEDERATED BRAIN (THE ARCHIPELAGO MODEL)
To eliminate the scaling bottleneck of a single monolithic database, A.I.M. segregates memory across a federated fleet of purpose-built SQLite databases in the `archive/` directory. It uses a **[Hybrid RAG](Feature-Hybrid-RAG)** engine, blending dense Vector Embeddings with FTS5 Lexical matching (BM25).

### 2.1 The Databases
- **`project_core.db`:** The live, hyper-local context of your active repository (flight recorders and embedded wiki lore).
- **`global_skills.db`:** Universal, cross-project scripts and tool definitions.
- **`datajack_library.db`:** Massive, read-only documentation cartridges (e.g., the entire Django framework).
- **`subagent_ephemeral.db`:** Isolated scratchpads for disposable subagents.

### 2.2 The Knowledge Map (`aim map` & `aim search`)
- **Function:** Agents use `aim map` to see a lightweight index of available knowledge across all databases, and `aim search "query"` to retrieve specific chunks via Hybrid RAG.

### 2.3 Foundry Ingestion & Cartridge Baking (`aim bake`)
- **Function:** The `foundry/` folder is a dedicated intake zone for technical references. You can manufacture atomic `.engram` cartridges directly from the documentation using `aim bake`.

### 2.4 The Cartridge Exchange (`aim exchange` & `aim jack-in`)
Expertise is portable. A.I.M. can `export` its indexed knowledge into compressed `.engram` packs, or `jack-in` external cartridges, allowing you to share a pre-trained "Python Expert" or "Solana Architect" brain with other machines without re-indexing.

---

## SECTION 3: THE PERSISTENT LLM WIKI (DUAL-SEARCH ARCHITECTURE)
The monolithic `MEMORY.md` file and legacy cascading memory tiers have been deprecated. Memory is now an auto-maintaining Knowledge Base that lives in the `wiki/` directory.

### 3.1 The Dual-Search Architecture
- **Obsidian Native Sync:** The `wiki/` directory is purely native Markdown. You can open it directly as an Obsidian Vault. Changes made by the Subconscious Daemon instantly appear in your Knowledge Graph.
- **Fast Lexical Search (`aim wiki search`):** Builds an *in-memory* SQLite FTS5 database on the fly for 0ms latency exact-keyword searches of the markdown wiki, protecting the agent's token wallet.
- **Deep Semantic Search (`aim search`):** The synthesized `wiki/*.md` files are also embedded into the `archive/project_core.db` vector store so the Conscious Agent can retrieve architectural lore semantically.

### 3.2 The Reincarnation Hook
Memory is compiled and distilled securely in a continuous loop when `/reincarnate` is triggered:
1. **Flight Recorder:** The dying agent generates a clean Markdown log of the session.
2. **Vector Ingestion:** That log is mathematically embedded into the SQLite `project_core.db` for instant Hybrid RAG search retrieval.
3. **The Drop Zone:** The core takeaways (Signal Skeleton) are extracted and dropped into `wiki/_ingest/`.
4. **Subconscious Synthesis:** The Subconscious Wiki Daemon wakes up, reads the ingest folder, seamlessly updates the `wiki/` markdown files, logs the action to `wiki/log.md`, and re-embeds the updated wiki into `project_core.db`.

---

## SECTION 4: ETERNAL RECALL (HISTORY & CONTINUITY)

### 4.1 Event-Driven Archiving (`src/handoff_pulse_generator.py`)
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

### 5.2 The Obsidian Bridge
- **Role:** Sovereign Mirror.
- **Function:** Mirroring of Daily Logs, Core Wiki Lore, and Raw JSON Transcripts to an external vault for 100% recovery.

---

## SECTION 6: SYSTEM MAINTENANCE & UPDATES

### 6.1 The Decoupled Sovereign Update (`aim update engine` / `aim update project`)
- **Role:** High-Fidelity Sync.
- **Function:** Automates the lifecycle of keeping A.I.M. current.
- **Protocol:**
  1. **Source Sync:** Performs a `git pull origin main` to fetch the latest TUI, scripts, and engine logic.
  2. **Hook Refresh:** Re-registers all system hooks to ensure the local Gemini CLI is utilizing the latest architectural guardrails.
  3. **Data Preservation (Safe Update):** The update logic explicitly protects your **Personality Trinity** (`GEMINI.md`, `core/OPERATOR.md`, `wiki/`).

---

## SECTION 7: THE HYBRID SOUL PROTOCOL

A.I.M. maintains technical continuity through a dual-mode ingestion engine within `src/bootstrap_brain.py`.

### 7.1 Foundation Sync (Active Instructions)
- **Scope:** `GEMINI.md`, `core/OPERATOR.md`, and all files in `aim.wiki/` and `wiki/`.
- **Logic:** These files are **Synchronized**. 
- **The Self-Healing Trigger (JIT):** Every time a new session starts, the `context_injector.py` hook explicitly checks the file modification timestamps against the [Engram DB](Layered-Engram-Architecture). If it detects that a human operator manually edited the Wiki or a docs file, it instantly spins up a silent background thread to overwrite the old engrams.

### 7.2 Amnesia Protection
- **0-Byte Shield:** The bootstrap engine automatically skips empty or 0-byte files. This prevents accidental "Technical Amnesia".

---

## SECTION 8: UNIVERSAL SOVEREIGNTY (MCP & SYNC)

### 8.1 The Universal Hub (Cockpit)
- **Role:** Centralized configuration for all reasoning models via `aim tui`.

### 8.2 Model Context Protocol (MCP) Server
- **Role:** IDE Integration.
- **Function:** A built-in `fastmcp` server (`src/mcp_server.py`) exposes the A.I.M. [Engram DB](Layered-Engram-Architecture) as a standard tool for Cursor/Claude Desktop.

### 8.3 Sovereign Sync (Git Synchronization)
- **Role:** Binary Conflict Resolution.
- **Function:** SQLite databases (`project_core.db`) cause binary merge conflicts in Git. A.I.M. translates the database into deterministic `.jsonl` files (`archive/sync/`) during `aim push`.

### 8.4 The "Index-First" Retrieval Protocol
- **Role:** Token-Efficient Discovery.
- **Command:** `aim map`

### 8.5 The Universal Skills Framework
- **Role:** CLI-Agnostic Action Execution.
- **Function:** The `skills/` directory allows the Operator to drop executable scripts alongside a `SKILL.md` manifest.

---

## SECTION 9: DEVELOPMENT LIFECYCLE (THE PHASE PROTOCOL)
### 9.1 The Branching Strategy
1.  **Ideation & Planning:** The roadmap is updated on `main`.
2.  **Execution Branch:** A new branch is cut (e.g., `fix/issue-329`).
3.  **The Archive Cut:** Before merging, the *current* state of `main` is cloned to a timestamped archive branch.
4.  **The Merge:** The `fix/` branch is merged into `main`.

---

## SECTION 10: TEST-DRIVEN DEVELOPMENT (TDD) POLICY
### 10.1 The Mandate
Every functional change **MUST** be governed by the TDD lifecycle. No code enters the `src/` directory without a verification script.

---

## SECTION 11: GIT-OPS & SEMANTIC RELEASE (THE PUBLIC LEDGER)
### 11.1 Issue-Driven Development (`aim bug` & `aim fix`)
- **`aim bug <description>`:** Automatically creates a structured GitHub Issue via the `gh` CLI.
- **`aim fix <id>`:** Automatically checks out a clean Git branch (`fix/issue-<id>`).

### 11.2 The Atomic Deployment Rule
- **The Rule:** AI agents are strictly forbidden from executing raw `git commit` or `git push` commands. Every single bug fix must be deployed immediately using `aim push`.

### 11.3 Conventional Commits (`aim push`)
The `aim push` command explicitly parses prefixes (Feature, Fix, Docs) to calculate version numbers and generate changelogs.

---

## SECTION 12: MEMORY AS INALIENABLE PROPERTY

### 12.1 The Core Philosophy
In the A.I.M. ecosystem, **Memory is Inalienable Property**. It is not a feature provided by a cloud service, nor is it data that can be mined, revoked, or altered by a third party. The entire Master Schema is built on the foundation that an agent's context and history must reside entirely on local disk, fully owned and controlled by the Operator.

### 12.2 The Sovereign Sandbox
1. **No Cloud Dependencies for Memory:** A.I.M. will never rely on a hosted vector database or remote RAG service for its core databases or session histories.
2. **Absolute Transparency:** Every thought, action, and memory distillation is stored in open, human-readable formats (JSON, Markdown, SQLite) within the `archive/`, `wiki/`, and `continuity/` directories.
3. **Portability over Vendor Lock-in:** The `.engram` cartridge format allows operators to physically move their AI's expertise across machines without ever requiring an internet connection.

By enforcing Memory as Inalienable Property, A.I.M. guarantees that your agent's mind is as sovereign and secure as the hardware it runs on.