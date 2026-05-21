# The Subconscious Node: Zero-API Setup Guide

> **CORE THESIS (Phase 33):** A.I.M. uses a **Distributed Cognitive Architecture**. While A.I.M. can natively run monolithically (handling all tasks on your main machine), you have the *optional feature* to offload the heavy computational lifting of wiki synthesis and vector chunking to a dedicated secondary machine: The Subconscious Node (The Brain PC). This keeps frontline dev agents lean, fast, and focused entirely on code generation.

Rather than building complex REST APIs or dealing with Git merge conflicts, A.I.M. uses **Obsidian Sync** (or Syncthing) as a secure, "Zero-API" transport layer to pass cognitive state between machines.

### ⚠️ The Local LLM Limitation
Our current architecture explicitly keeps everything **native within `gemini-cli`** utilizing the Google Gemini API. We intentionally ripped out the brittle Python regex webhook parsers that tried to force local models to format massive strings.
You might wonder why we don't just point the `gemini-cli` natively at a local Ollama or DeepSeek server using an OpenAI-compatible endpoint. Unfortunately, the `gemini-cli` core maintainers have explicitly rejected PRs attempting to add OpenAI-compatible routing (see [PR #1975](https://github.com/google-gemini/gemini-cli/pull/1975) and [PR #5362](https://github.com/google-gemini/gemini-cli/pull/5362)). They are committed to restricting the CLI exclusively to the Gemini model ecosystem. 
Until the team adds native support for other APIs, A.I.M.'s Subconscious Node will securely utilize a dedicated `tmux` session running `gemini --yolo` to execute its tasks. This ensures flawless, tool-driven native file interactions without the fragility of string parsing.

---

## 🧠 The Three Cognitive Modes
When configuring an A.I.M. instance via the TUI (`aim config`), you can set the machine to one of three roles:

1. **Monolithic (Default):** The machine does everything. It writes code and refines its own memory locally on your primary LLM. This is the standard operational mode.
2. **[Frontline Agent](Remote-Fleet-Architecture):** The primary coding laptop. It generates chat transcripts but *bypasses* the token-heavy Wiki pipeline. It drops its extracted "Signal Skeletons" into the synced Obsidian Vault and waits for the Brain to process them.
3. **Subconscious Node (The Brain):** A background server. It does not write code. It strictly monitors the Obsidian Vault, crunches the incoming skeletons using a dedicated `wiki_agent` tmux session, and flawlessly synthesizes them into the Permanent LLM Wiki.

---

## 🛠️ Step-by-Step Architecture Setup

### 1. Establish the Transport Layer (Obsidian Vault)
Both machines must have access to the exact same directory. Since the Persistent LLM Wiki is natively Markdown, the `wiki/` folder itself serves as the Vault.
1. Map your Syncthing (or Obsidian Sync) to ensure the `wiki/` folder is perfectly mirrored between the Frontline laptop and the Subconscious server.
2. Ensure the `wiki/_ingest/` drop zone exists.

### 2. Configure the Frontline Agent (Your Laptop)
1. Open the terminal in your project workspace.
2. Run `aim config`.
3. Set the **Obsidian Vault Path** to the local mirrored folder.
4. Set the **Cognitive Architecture** to `frontline`.

*Behavior:* When you run `/reincarnate`, this agent will bypass the LLM distillation. It simply embeds the raw transcript into its local `memory_lance` and drops a basic Markdown summary directly into `wiki/_ingest/`.

### 3. Configure the Subconscious Node (The Server)
1. SSH into your secondary machine or spare computer.
2. Install the A.I.M. CLI (`git clone https://github.com/BrianV1981/aim.git && cd aim && aim init`).
3. Run `aim config`.
4. Set the **Obsidian Vault Path** to the server's mirrored folder.
5. Set the **Cognitive Architecture** to `subconscious`.

### 4. Ignite the Brain Daemon
On the Subconscious Node, you initiate the background process by running:
```bash
aim daemon start
```

*Behavior:* The daemon will silently run in the background. It watches `wiki/_ingest/` for `.md` files. The moment Syncthing drops a new Markdown skeleton from the frontline agent into that folder, the daemon wakes up, automatically spawns a `wiki_agent` tmux session running `gemini --yolo` (if it isn't running already), and natively hands off the files to the agent via tmux buffer pasting. The agent will read the files, elegantly weave the new knowledge into the existing Markdown wiki files (e.g., `wiki/index.md`), and delete the ingested files.

Syncthing then automatically beams those synthesized Wiki pages *back* to your Frontline laptop, where your agent natively inherits the new knowledge.

---

### The Sovereign Mirror 
Because the transport layer is an Obsidian Vault, you (the human operator) can open the Vault in the Obsidian application at any time. You will see a beautiful, interactive graph of your project's memory being generated in real-time by the Subconscious Node's `wiki_agent`, and you can edit any hallucination by simply typing in the markdown files.