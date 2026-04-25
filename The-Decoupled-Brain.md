# The Decoupled Brain & Obsidian Transport Layer

> ⚠️ **STATUS: ACTIVE ARCHITECTURE**
> *This document outlines the architectural pivot from a monolithic local agent to a distributed cognitive system utilizing Obsidian as a native transport layer.*

## The Monolithic Bottleneck

Currently, many AI agents use a **monolithic architecture**. The "Hands" (the active coding agent manipulating the terminal) and the "Brain" (the LLM tasked with synthesizing the memory pipeline) run on the exact same machine, within the same event loops, and draw from the same API quotas and rate limits.

This creates several fatal bottlenecks:
1. **API Exhaustion:** If the background daemon attempts to summarize 50 turns of history, it can trigger a `MODEL_CAPACITY_EXHAUSTED` (429) rate limit, instantly paralyzing the active coding agent in the primary terminal.
2. **Compute Contention:** Running heavy LLM summarization prompts drains local GPU/CPU resources, slowing down the active development environment.
3. **The Cleanup Problem:** RAG databases suffer from "ghost context" because deleting specific concepts from a compiled vector database is incredibly difficult without wiping the entire table.

## The Paradigm Shift: Decoupling Control & Data

To scale, A.I.M. decouples the **Control Plane** (The Conscious coding agent) from the **Data Plane** (The Subconscious Wiki Daemon). 

We can offload the Subconscious Brain to a dedicated background server (e.g., a spare Mac Mini, a Raspberry Pi, or a cheap Cloud VPS), allowing it to synthesize the Persistent LLM Wiki 24/7 on separate API keys or local hardware without ever interrupting the developer.

### The Problem of Networking (The Merge Conflict)
Decoupling introduces a major networking challenge. If Machine A (The Hands) and Machine B (The Brain) are constantly reading and writing to the same databases, we will encounter severe race conditions. Building a custom REST API, WebSocket server, or managing firewall rules and SSL certificates to handle this synchronization introduces massive, unnecessary overhead.

## The Solution: The Obsidian Transport Layer

Rather than building a custom networking layer, A.I.M. uses **Obsidian Sync** (or Syncthing) as a secure, "Zero-API" transport layer.

### The Great Transport Debate: GitHub vs. Obsidian
While a private GitHub repository *could* theoretically serve as a transport bridge between machines, it violates A.I.M.'s sovereign philosophy for handling raw memory.

1. **The Security Vector (E2EE):** GitHub requires trusting Microsoft's cloud with raw, unredacted thought streams. Every hallucination or accidental API key slip-up becomes version-controlled on a remote server. Syncthing uses strict, military-grade **End-to-End Encryption (E2EE)** directly between your peers. 
2. **The Commits of Chaos:** Git is designed for immutable version history. A continuous memory pipeline generates thousands of micro-updates. Obsidian treats files as fluid state.
3. **The Sovereign Mirror:** Obsidian provides a native Graph View. By using it as the transport layer, the Operator gains a beautiful, interactive Graphical User Interface (GUI) to explore and surgically edit the agent's subconscious in real-time.

*Git is for deploying Code. Obsidian is for syncing Cognitive State.*

### The Cross-Machine Workflow:

1. **The Hands (Machine A):** 
   The developer finishes a coding session. The A.I.M. `/reincarnate` hook embeds the raw session vector into `archive/project_core.db` locally, but drops the "Signal Skeleton" (takeaways) into the `wiki/_ingest/` folder.

2. **The Transport:** 
   Because the entire `wiki/` directory is mapped as an Obsidian Vault, Syncthing automatically detects the new `_ingest/` files and beams them peer-to-peer over an encrypted tunnel to the secondary machine.

3. **The Brain (Machine B):** 
   A headless A.I.M. daemon runs continuously on the server. When the new skeleton arrives in `wiki/_ingest/`, the Subconscious Wiki Daemon wakes up, reads the file, and elegantly synthesizes the new architectural decisions into the native Markdown wiki files (e.g., `wiki/index.md`).

4. **The Return:** 
   Syncthing automatically beams the newly updated `wiki/*.md` files back to the developer's primary coding machine.

5. **The Rebuild:** 
   When the coding agent needs to search its memory, it uses `aim wiki search` for instant 0ms lexical lookup of the synced files, and the background indexer ensures the new Wiki lore is automatically embedded back into Machine A's `project_core.db` for deep semantic RAG.

### Why this architecture wins:
* **Zero Custom Networking:** No open ports, no complex API routing, no auth tokens to manage between devices.
* **Effortless Cleanup:** Because the `wiki/` is plain text, "deleting a memory" simply means deleting a line in a `.md` file.
* **The Visual Graph:** As a free byproduct of this architecture, the user can open their A.I.M. workspace in the Obsidian application and see a beautiful, interactive, real-time graphical representation of their project's entire subconscious memory.