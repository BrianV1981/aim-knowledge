# The Persistent LLM Wiki Architecture

> 💡 **ORIGIN CREDIT:** This architecture is deeply inspired by Andrej Karpathy's brilliant ["LLM Wiki" concept (view the original Gist here)](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). Karpathy proposed that instead of treating LLMs as dumb RAG retrievers that rediscover everything from scratch, we should let the LLM build and maintain a persistent, interlinked markdown wiki—a living, compounding knowledge base.

A.I.M. has taken this concept and integrated it natively into our multi-agent, GitOps-driven exoskeleton. However, we recognized a critical UX bottleneck: **forcing a "Conscious Agent" (the one the operator is actively using to write code) to stop, read, cross-reference, and rewrite 15 markdown files creates terrible latency and token burn.** 

To solve this, A.I.M. implements the LLM Wiki as a strictly delineated, **Event-Driven Subconscious Pipeline**.

---

## 1. The Delineation: RAG vs. The Wiki

A.I.M. explicitly separates *reference data* from *synthesized logic*.

**The Engram DB (Hybrid RAG / Semantic & Lexical Search)**
*   **The Content:** Immutable, massive, high-volume reference data.
*   **Examples:** DataJack `.parquet` cartridges (entire frameworks like React, Django, or Rust), raw API specifications, massive server error logs, and raw third-party codebases.
*   **The Logic:** You don't want the AI trying to "synthesize" or summarize the Django framework. You just want it to instantly retrieve the exact syntax for `HttpResponseRedirect` in milliseconds when it needs it. RAG is perfect for finding a needle in a static haystack.

**The LLM Wiki (Persistent Markdown)**
*   **The Content:** Mutable, highly synthesized, project-specific knowledge and evolving logic.
*   **Examples:** Your architectural decisions, specific business logic, active feature roadmaps, competitor analysis, and most importantly, your **Eureka Axioms** (the lessons learned from fixing bugs).
*   **The Logic:** This is the *meaning* extracted from your daily work. If you ask, "Why did we choose SQLite over PostgreSQL?", you don't want the AI doing a vector search across 50 chat logs to reconstruct the argument on the fly. You want it to read `wiki/Database-Architecture.md` where the Subconscious LLM has already beautifully summarized the decision, the trade-offs, and the date it was made.

---

## 2. The Offloading Pipeline (The "Subconscious" Fleet)

A.I.M. architects the offloading so your primary agent (and your wallet) never feels the burden of wiki maintenance.

### Step A: The Ingestion Drop Zone (`wiki/_ingest/`)
We create a dedicated, physical folder at `wiki/_ingest/`. 
*   **Manual Drops:** You can manually drag and drop a PDF about a new competitor, a text file with a shower thought, or a raw API response into this folder.
*   **Automated Drops:** When a heavy coding session ends, the `aim_reincarnate.py` script extracts the "Signal Skeleton" and executes a Direct Python Handoff (via `subprocess.Popen`), spawning the `session_summarizer.py` daemon in the background to drop the skeleton into this folder.
The Conscious Agent simply hands off the context and goes to sleep. Zero latency for the operator.

### Step B: The Bridge (Syncthing)
Because your project folder is synced (via Syncthing or Obsidian Sync), that raw file in `wiki/_ingest/` immediately syncs over to your secondary PC or home GPU server.

### Step C: Event-Driven Offloading (Direct Python Handoff)
We absolutely avoid fragile webhook pings, background cron-jobs, or infinite file-watching loops (which drain CPU and invite silent hallucinations). The process is strictly **Event-Driven** and decoupled from the active CLI session.
*   When `/reincarnate` is executed, the dying agent triggers the `aim_reincarnate.py` script.
*   The script spawns the `session_summarizer.py` scribe as a detached background daemon (`--bg` flag).
*   The daemon wakes up completely independently of the main thread, processes the synced `wiki/_ingest/` folder, and goes to work using a dedicated `wiki_agent` tmux session.

### Step D: The Subconscious Execution (The Native Tmux Agent)
Instead of forcing a local LLM to output massive formatted text strings and parsing them with fragile Python regex, the Subconscious Scribe leverages the A.I.M. Swarm philosophy. It automatically spins up a background `tmux` session named `wiki_agent` running `gemini --yolo` and pastes in a directive to update the wiki.
Because the official `gemini-cli` core maintainers have explicitly rejected PRs to add native OpenAI-compatible local model routing (see [PR #1975](https://github.com/google-gemini/gemini-cli/pull/1975) and [PR #5362](https://github.com/google-gemini/gemini-cli/pull/5362)), we intentionally keep the Subconscious Node within the native Gemini ecosystem to avoid parsing fragility.
Once the `wiki_agent` wakes up, it executes a highly deterministic pipeline using native tools:
1.  **Read:** It uses `read_file` to read the first file in `wiki/_ingest/`.
2.  **Lint/Map:** It reads the `wiki/index.md` to see what concepts already exist.
3.  **Synthesize:** It uses `write_file` or `replace` to natively edit the markdown pages, seamlessly weaving the new knowledge into the project's lore/architecture.
4.  **Log:** It natively appends a one-line entry to `wiki/log.md`.
5.  **Clean:** It uses `run_shell_command` to delete the raw file from `wiki/_ingest/` so it never processes it twice. When the queue is empty, the agent waits for the next buffer paste.

### Step E: The Return & The Dual-Search Architecture
The new system will operate on a "Dual-Search" architecture to maximize speed and semantic understanding:
- **Obsidian Native Sync:** The entire `wiki/` directory is purely native Markdown. This means you can open the `wiki/` folder directly as an Obsidian Vault. Any changes made by the Subconscious Daemon will instantly appear in your Obsidian knowledge graph.
- **Fast Lexical Search (`aim wiki search`):** We use the `wiki_tools.py` logic to build an *in-memory* Tantivy index on the fly. This provides 0ms latency exact-keyword searches of the markdown files without needing to re-index them.
- **Deep Semantic Search (`aim search`):** To ensure the Conscious Agent can "feel" the architectural decisions via vector embeddings, the synthesized `wiki/*.md` files are ingested into the `memory_lance` vector store alongside the raw session flight recorders.

### Step F: The Reincarnation Handoff
When you execute `/reincarnate`:
1. The dying agent generates a clean Markdown log of the session (The Flight Recorder).
2. That log is mathematically embedded into the native LanceDB memory pool for instant Hybrid RAG search retrieval.
3. The core takeaways are dropped into `wiki/_ingest/` via the Direct Python Handoff background daemon.
4. The `wiki_agent` tmux session wakes up, reads the ingest folder, updates `wiki/index.md` with the new lore, and gracefully exits.
5. The updated Wiki pages are re-embedded directly into `memory_lance` so the main `aim search` command can find them semantically.

---

## 3. Guaranteeing Epistemic Certainty (Avoiding Knowledge Drift)

If an agent has to read a 1,500-word philosophical essay every time it wakes up just to figure out how to log a note, it will burn tokens and hallucinate. To ensure the system is perfectly understood every single time an agent fires up, we split the instructions into two hardcoded, deterministic places:

### The Conscious Agent's Rule (`AGENTS.md`)
Your primary agent does not need to know *how* to maintain a wiki. It only needs to know how to read it, and how to dump data into it. 
When a user runs `aim init` and opts into the Wiki system, A.I.M. injects one simple, strict rule into their core `AGENTS.md` guardrails:

> **9. THE PROJECT WIKI (LONG-TERM MEMORY)**
> - **To Read:** The project's synthesized lore and architecture live in the `wiki/` folder. Always start by reading `wiki/index.md`.
> - **To Write:** DO NOT manually edit the wiki pages. If you learn a new "Eureka" moment or have a new document to add, write the raw text file into `wiki/_ingest/` and it will automatically be handed off to the Subconscious Daemon during reincarnation.

### The Subconscious Daemon's Rule (`WIKI_SCHEMA.md`)
The local LLM running on your secondary PC that actually does the formatting needs strict rules. 

When you run `aim init`, the setup wizard asks for the name and scope of your wiki (e.g., "Project Nebula Lore"). It uses this to generate a file called **`wiki/WIKI_SCHEMA.md`**. 

This is the **System Prompt** for the background daemon. Every time the daemon wakes up to process the `_ingest/` folder, it loads this exact file:

```markdown
# SYSTEM PROMPT: WIKI MAINTAINER
You are the Subconscious Wiki Daemon for "Project Nebula Lore".
Your job is to read files in the `_ingest/` folder and seamlessly integrate them into this markdown wiki.

**RULES:**
1. Always update `wiki/index.md` if you create a new page.
2. Always append a one-line timestamped summary of your actions to `wiki/log.md`.
3. Never delete existing factual context; synthesize new contradictions dynamically.
4. Output your changes as raw markdown file writes.
```

By turning the philosophy into hardcoded GitOps rules and distinct system prompts, we guarantee 100% Epistemic Certainty for both the Conscious and Subconscious agents every time they wake up.ake up.