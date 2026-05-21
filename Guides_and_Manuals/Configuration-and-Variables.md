# Configuration & Variables Overview

Almost every behavior in A.I.M. is driven by a configurable variable exposed in the TUI (`aim tui`). A.I.M. is fundamentally an operating system for your AI, and it gives you complete control over how the intelligence is routed, governed, and constrained.

## ⚙️ Bring Your Own LLM (Configuring Providers)
A.I.M. is **Provider-Agnostic**. While Ollama is the default for free, offline local processing, you are not locked into it. The `aim tui` interface allows you to swap models, providers, and endpoints for both the Conscious Agent and the Subconscious Daemon.

### 1. LLM & Cognitive Routing (Per-Tier Customization)

> **Important Note on the "Primary Brain":** The interactive chat model you talk to daily is governed exclusively by your global Gemini CLI settings (`~/.gemini/settings.json`). The "Primary Brain" configured in the `aim tui` serves as an explicit fallback model for headless background scripts (like `aim audit`, `aim scrape`, or crash recoveries) that execute autonomously under the hood.

*   **Providers:** Local (Ollama/Llama.cpp), Google Gemini API, OpenAI, Anthropic, Custom OAuth.
*   **Models:** Swappable per role (e.g., `gemini-3.1-pro-preview` for the Conscious Agent, `gemma4:e4b` for the Subconscious Daemon/Tier 1).
*   **Endpoints & Auth:** Custom API URLs and Auth Types (OAuth, API Key, None).
*   **Embeddings:** Swappable embedding models and endpoints. While `nomic-embed-text` running locally is the free default, you can completely elevate your RAG performance by swapping to a premium remote API like [Gemini Embedding 2](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-embedding-2/).

---

## 🛡️ Guardrails & Anti-Drift (Cognitive Mantra)
The Executive Guardrails actively monitor the agent's behavior to prevent context drift and rogue execution.

*   **`cognitive_mantra.enabled`:** Toggle the anti-drift system on or off.
*   **`whisper_interval`:** How many tool calls before a silent reminder is injected into the agent's context.
*   **`mantra_interval`:** How many tool calls before a hard halt and forced recitation of the GitOps rules.

---

## 🧠 Execution & Context Limits
A.I.M. manages the context window and lifecycle of the agent using configurable limits.

*   **`handoff_context_tail`:** Number of turns preserved in the dead-man's switch snapshot when generating a handoff.
*   **`auto_rebirth`:** Whether the agent automatically spawns its successor upon context collapse (Reincarnation).
*   **`archive_retention_days`:** How long to keep historical sessions and artifacts.
*   **`cleanup_mode`:** Choose between `ARCHIVE` or `DELETE` for consumed files.

---

## 🌐 The Sovereign Swarm (P2P)
Control how your local node interacts with the global decentralized intelligence network.

*   **`swarm_enabled`:** Toggle BitTorrent DataJack tracking to share or download `.parquet` cartridges.
*   **`seeding_ratio` and `max_download_speed`:** Traffic shaping and bandwidth limits for the swarm.

---

## 🎭 Agent Persona & Architecture
You can configure the high-level identity and operational structure of the agent.

*   **Operator Profile:** Set execution mode, cognitive level, and conciseness to tune how the agent interacts with you.
*   **Cognitive Architecture:** Switch between Monolithic, Frontline, or Subconscious modes.
*   **Obsidian Vault Path:** Configure the local path for the two-way Obsidian Bridge sync.

*Launch the cockpit anytime by running `aim tui` to adjust these settings.*