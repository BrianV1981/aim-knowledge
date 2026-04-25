# A.I.M. (Actual Intelligent Memory)

**"Treat your AI like a bot, not an oracle. Built by a gamer, for the trenches."**

☕ **Support the project:** [Buy Me a Coffee](https://buymeacoffee.com/brianv1981)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/Version-1.7.0-blue.svg)](https://github.com/BrianV1981/aim/blob/main)
[![Platform](https://img.shields.io/badge/Platform-Gemini%20CLI-0088cc.svg)](https://github.com/BrianV1981/aim/blob/main)

**A.I.M.** is an open-source engineering OS designed to cure the "Amnesia Problem" of autonomous AI agents.

---

> ⚠️ **ALPHA STATUS & THE [VIBE CODING](Benchmark-Vibe-Coding) ORIGIN**
>
>A.I.M. wasn't built by a machine learning researcher in Silicon Valley. It was built by a gamer who spent years writing macros and automation bots for MMOs. Asheron's Call, Star Wars Galaxies, SWTOR, and anything else with repetitive clicking or a second account that needed a buff bot.
>
> When you automate an MMO character, you learn quickly that you can't rely on "magic." You need rigid guardrails (GitOps/TDD), clearly defined scopes (roadmaps/plans/issue tickets), inventory management (context), and watchdog timers (context size) to reset the bot when it inevitably wanders off path. A.I.M. applies this exact philosophy to LLMs. Instead of trying to build a smarter brain, A.I.M. provides the operating system.
>
> **This project is radically transparent.** It is an Alpha Proof-of-Concept built entirely via "[vibe coding](Benchmark-Vibe-Coding)" — using the very exoskeleton it provides. The commit/issue history shows the iterations, the bug storms, the live patches against real API outputs. There is no VC-funded black box here ***cough*** Cursor ***cough***. The plumbing is being battle-tested in public, and the architecture proves that a structured AI workflow can turn a non-coder into a systems architect.

📖 **Full Documentation:** [Read the Official GitHub Wiki](https://github.com/BrianV1981/aim-wiki/wiki)
🚀 **Getting Started:** [Read the Step-by-Step User Guide](https://github.com/BrianV1981/aim-wiki/wiki/User-Guide)
⚙️ **Configuration:** [Configuration & Variables](Configuration-and-Variables)
🎓 **Day 1 Tutorial:** [Build Your First Feature (Quickstart)](https://github.com/BrianV1981/aim-wiki/wiki/Quickstart-Tutorial)
🛠️ **Engineers:** Hate biological metaphors? [Read the metaphor-free Technical Spec here](https://github.com/BrianV1981/aim-wiki/wiki/Technical-Specification).

---

## 🛑 The Problem: The 50-Turn Spaghetti Code

If you let an AI agent code autonomously, it is brilliant for the first 10 turns. By turn 50, its context window is overflowing, it forgets your architectural rules, it hallucinates answers, and your repository degrades into unmaintainable spaghetti code.

The industry's answer is to build a bigger context window. That's giving a distractible worker a bigger desk.

A.I.M.'s answer is a strict Standard Operating Procedure, a memory sieve, and a leash.

---

## 🚀 The Solution: A Sovereign Exoskeleton

A.I.M. wraps around your CLI agent (Gemini CLI/GPT Codex/Claude Code) and forces it to act like a disciplined Principal Engineer. It externalizes the agent's memory into a local SQLite database, automates its Git lifecycle, and prunes its thoughts while you sleep.

---

## 🧠 The Architecture (v1.7)

A.I.M. does not use a standard RAG pipeline. Its core insight is separating **what requires an LLM** from **what doesn't** — and ruthlessly offloading everything that doesn't.

### 1. The Zero-Token Python Engine (The Autonomic Layer)

This is A.I.M.'s primary architectural breakthrough, and it's what most memory frameworks miss entirely.

**The AI industry assumes memory management requires an LLM.** A.I.M. doesn't. The Tier 1 Harvester uses pure, deterministic Python — no LLM, no API calls, zero tokens — to strip raw terminal JSON noise down to a clean "Signal Skeleton," reducing token weight by up to 85% before an LLM ever touches the data.

*   **Empirical Proof (The Noise Filter):** When processing the raw, successful [`matrix_pro_run1_final.json`](benchmarks/raw_logs/matrix_pro_run1_final.json) benchmark log, the native `scripts/extract_signal.py` script reduced the file from **269,537 characters (264 KB)** of chaotic JSON down to a pristine **56,325 characters (56 KB)** of human-readable [Markdown](benchmarks/raw_logs/MATRIX_GEMINI_RUN1.md). That is a **79.1% reduction** in context weight, executed in milliseconds, for exactly $0.00.

Background scripts (`failsafe_context_snapshot.py`) silently index data, capture checkpoints, and scrub secrets. The AI doesn't think about any of this, just like you don't think about breathing.

**The Universal Hook Router:** A.I.M. employs a context-aware global router dispatch. This means you can run 5 different agents in 5 different project directories simultaneously. The router automatically detects the active terminal and routes the memory pipeline to that specific local `engram.db`, ensuring **zero cross-contamination** between concurrent workspaces.

**The Executive Guardrails (Anti-Drift):** A.I.M. actively monitors the agent's behavior via the `cognitive_mantra` hook. It tracks autonomous tool calls. At 25 actions, it injects a silent "Subconscious Whisper" to remind the agent of its mandates. At 50 actions, it forcefully halts execution and requires the agent to output a `<MANTRA>` block reciting its GitOps rules, completely washing away "Lost in the Middle" context degradation.

### 2. The Persistent LLM Wiki (The Dual-Search Brain)

The legacy 4-tier cascading memory and the monolithic `MEMORY.md` file have been completely demolished. Memory is now handled via the **[Persistent LLM Wiki Architecture](The-Persistent-LLM-Wiki-Architecture)** and a Dual-Search engine.

When a coding session concludes, the Reincarnation pipeline wakes up and does two things instantly:
1. **Raw Vector Ingestion:** It mechanically embeds a noise-reduced flight recorder of the session directly into the SQLite `project_core.db` for zero-latency semantic retrieval via Hybrid RAG.
2. **Subconscious Synthesis:** It drops the core takeaways into a `wiki/_ingest/` drop zone. A background LLM Daemon wakes up, reads the ingested notes, and elegantly synthesizes the new architectural decisions into a multi-file Markdown Wiki. No more monolithic files. Just an ever-evolving, beautifully interlinked knowledge graph.

**The Federated Brain (The Archipelago Model):** 
To eliminate the scaling bottleneck of a single monolithic database, A.I.M. segregates memory across a federated fleet of purpose-built SQLite databases:
*   **`project_core.db`**: The live, hyper-local context of your active repository (flight recorders and embedded wiki lore).
*   **`global_skills.db`**: Universal, cross-project scripts and tool definitions.
*   **`datajack_library.db`**: Massive, read-only documentation cartridges (e.g., the entire Django framework).
*   **`subagent_ephemeral.db`**: Isolated scratchpads for disposable subagents.

**The Obsidian Bridge & Remote Fleet:** 
Because the ultimate source of truth is a pure, native Markdown Wiki directory (`wiki/`), A.I.M. allows you to offload the heavy LLM memory jobs to a secondary server. You simply sync the project folder to an Obsidian Vault, and a headless A.I.M. daemon on your local GPU rig watches the `wiki/_ingest/` folder, compiles the memory, and syncs the updated Wiki pages back to your primary laptop—giving you a real-time, visual Obsidian knowledge graph across machines.

### 3. The Subconscious (Modular Hybrid RAG)

The permanent memory lives in the **[Engram DB](Layered-Engram-Architecture)** — a local SQLite database with two key properties:

**Hyper-Modular Infrastructure:** The Engram system is not locked to any specific embedding model. It defaults to free local `nomic-embed-text` for zero-cost offline usage, but the architecture is designed to be instantly swappable. You can wire in cutting-edge multimodal models like [Gemini Embedding 2](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-embedding-2/) (supporting text, image, video, audio, and PDF) and rebuild your cartridges to drastically increase retrieval accuracy. The system is a fluid data stream, not a stone tablet.

**The Disposable Database:** The SQLite databases (like `project_core.db`) are not the sole source of truth — they are high-speed caches. The permanent memory lives in raw `.md` session logs in `archive/history/` and human-readable synthesized documents in the `wiki/` folder. If the database ever corrupts, `aim sync` rebuilds the vector index from scratch. The databases are disposable; the text files are eternal.

**The "Photograph" Effect:** A.I.M. uses Hybrid Retrieval — dense vector embeddings for abstract "vibe" searches, paired with FTS5 lexical matching for exact variable names and error codes. When you hit the database with a specific string, the full context floods back.

---

## 🔥 Killer Features

### 🔀 Modular Cognitive Routing (The "A La Carte" Brain)

This is A.I.M.'s primary economic advantage over generic IDE wrappers.

Cursor and Claude Code force every background task — every file summary, every memory index update — through an expensive flagship model. A.I.M. decouples the "subconscious" from the "conscious mind":

- **The Frontal Lobe:** Keep a flagship model (**Gemini 3.1 Pro** or **Opus 4.6**) as your active coding agent for complex reasoning.
- **The Subconscious:** Offload the heavy, repetitive background tasks (Tier 1 Harvester, Tier 2 Distiller) to fast, inexpensive models like **Gemini Flash**, **Sonnet**, or **Haiku**.
- **The Free Offline Brain:** Route the entire background memory pipeline to a local model (**Ollama gemma4:e4b** by default) on your GPU, or swap to any external API provider via `aim tui`. A.I.M. automatically injects explicit guardrails to prevent hallucination.

The result: flagship intelligence at the terminal, fraction-of-a-cent models doing the filing.

### 🎯 The Matrix Agent (Plug-and-Play Specialists)

The ultimate goal of A.I.M. is not a generalist IDE. It is a **Matrix Agent**.

By combining a specific knowledge cartridge (`django_master.engram`) with a surgically constrained `GEMINI.md` and a mandated workflow (forced `pytest` execution), you don't need the smartest model in the world. You need a model that can follow directions.

You don't rely on the LLM's pre-trained weights to know the answer. You inject the exact documentation, mandate the exact testing tools, and force the agent to loop until the terminal output is green.

### 🔌 The DataJack Protocol ("I Know Kung Fu")

Package 10,000 pages of documentation into a single [example `.engram` cartridge](https://github.com/BrianV1981/aim/releases/download/v1.7.0/python314.engram). When another developer runs `aim jack-in python.engram`, it executes a secure, parameterized SQLite insertion directly into their database. The agent wakes up with pre-calculated semantic recall of the entire library. Zero embedding calls. One person pays the compute tax once — the community inherits it.

```
> aim jack-in python314.engram
[1/2] Unpacking cartridge...
[2/2] Downloading Math (Nomic Embeddings) into Subconscious...
[SUCCESS] I know Kung Fu. (503 knowledge sessions injected in 3.1s)
```

### 🐙 The GitOps Bridge

A.I.M. forces [atomic](Atomic-Architecture), traceable deployments:

- **`aim bug "desc"`** — Creates a structured GitHub Issue with Commander's Intent and crash logs.
- **`aim fix <id>`** — Spawns an isolated Git Worktree (`workspace/issue-x`) for concurrency and TDD.
- **`aim push "msg"`** — Parses Conventional Commits, calculates SemVer bumps, auto-generates `CHANGELOG.md`.
- **`aim promote`** — Merges the isolated feature into `main` and automatically deletes the Worktree.

AI agents are forbidden from raw `git commit` or `git push`. Every change is tied to an issue. Every regression is instantly revertible.

### 🛡️ The Context Collapse Shield

When the Gemini CLI prunes history, A.I.M. intercepts the event and maintains a rolling "Failsafe Snapshot" of raw JSON logs. The Signal Skeleton reduces token weight by up to 85%, ensuring the agent retains critical architectural context even after compression. The "Bouncer" script detects `[EPHEMERAL]` tags on temporary subagents and bans their noise from the long-term database.

### 🛑 Predictable Restraint (Anti-Rogue Execution)
A massive danger with modern, highly capable LLMs is their tendency to "auto-execute." If you spin up a standard AI agent in a repository that contains a `TODO.md` or `TASK.md` file, it will often begin autonomously editing your code the second it boots up, without waiting for your permission. A.I.M.'s rigid GitOps and behavioral hierarchy physically suppresses this instinct. The agent treats the operator as the explicit trigger, ensuring it never touches your codebase until you give the exact execution order. *(See empirical proof of this in the [Matrix vs. Base Weights Benchmark](benchmarks/BENCHMARK_DJANGO_MATRIX.md)).*

### 🌐 Universal IDE Support (MCP)

The built-in MCP Server exposes the [Engram DB](Layered-Engram-Architecture) to any connected IDE — **Cursor**, **VS Code**, or **Claude Desktop** — without any platform-specific adapter code. We have also already begun working on an [Antigravity adaptation](https://github.com/BrianV1981/aim-antigravity).

---

## 🛠️ The Command Suite

- `aim init` — Setup wizard. Clean Sweep & Cognitive Guardrails.
- `aim tui` — The Cockpit. Configure multi-provider LLM routing & Swarm integration.
- `aim bug` — Creates a structured GitHub Issue with explicit Action Items.
- `aim fix` — Spawns an isolated Git Worktree (`workspace/issue-<id>`) for concurrency.
- `aim push` — Deploys changes atomically with Semantic Versioning.
- `aim promote` — Merges code to main and dissolves the temporary Worktree.
- `aim merge-batch` — Sequentially pulls and merges multiple remote GitOps branches.
- `aim search` — Sub-millisecond Hybrid semantic/lexical search across project history.
- `aim bake` — Compiles a directory of markdown files into an `.engram` cartridge.
- `aim export` — Seeds a local `.engram` cartridge to the P2P [Sovereign Swarm](The-Sovereign-Swarm).
- `aim jack-in` — Downloads and securely ingests an `.engram` magnet link.
- `aim ingest` — Pulls human edits from your Obsidian Vault back into the AI's Brain.
- `aim status` — Show current project momentum and open issues.
- `aim map` — See a lightweight index of all loaded documentation titles.
- `aim wiki` — Manage the Persistent LLM Wiki (search/process).
- `aim reincarnate` — Triggers the automated Context Handoff protocol for bloated agents.
- `aim crash` — Interactive Crash Recovery Protocol to salvage an interrupted V8 heap.
- `aim update engine` / `aim update project` — Safe decoupled updates for the A.I.M. exoskeleton or target project without losing local memory.

---

## 🏗️ Installation (Linux / WSL)

> **Note:** A.I.M. currently requires Linux or WSL (Ubuntu). macOS support is on the roadmap.

### 1. Prerequisites

```bash
# Remove restricted snap utilities
sudo snap remove curl
sudo apt update && sudo apt install -y curl git python3-venv libfuse2t64 xdg-utils

# Install NVM and Node 20+
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
source ~/.bashrc
nvm install 20 && nvm use 20

# Install Gemini CLI and authenticate
npm install -g @google/gemini-cli
gemini login  # CRITICAL: Do this before proceeding

# Install Ollama for local embeddings
curl -fsSL https://ollama.com/install.sh | sh
ollama pull nomic-embed-text
```

### 2. Install A.I.M.

```bash
git clone https://github.com/BrianV1981/aim.git
cd aim
./setup.sh
source ~/.bashrc
aim init
```

### 3. Configure

```bash
aim tui  # Set your LLM providers, routing, and guardrails
```

For the complete deployment manual including Obsidian Vault setup, see the [Installation Guide](https://github.com/BrianV1981/aim-wiki/wiki/Installation-Guide).

---

## 📖 Core Documentation

- [The Master Schema](https://github.com/BrianV1981/aim-wiki/wiki/The-Master-Schema) — Full architectural map
- [Technical Specification](https://github.com/BrianV1981/aim-wiki/wiki/Technical-Specification) — Metaphor-free engineering specs
- [Benchmarks](https://github.com/BrianV1981/aim-wiki/wiki/Benchmark-Render) — Repeatable, raw-data-backed comparisons

---

> *"Alcmaeon of Croton, Praxagoras of Kos, and Herophilus of Chalcedon were three Ancient Greek philosophers interested in the relation between the head and the body. Alcmaeon argued that the brain is the seat of intelligence, connected to the extremities of the body by poroi. Praxagoras suggested that the brain controls movement in the body, and posed the existence of neurons responsible for sending brain signals through the body. Herophilus used dissection to demonstrate the existence of a nervous system distinct from the vascular system, discovered nerves connected to inner organs and muscles, and distinguished between sensory and motor nerves."*, and distinguished between sensory and motor nerves."*