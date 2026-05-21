# Map: A.I.M. Prompt Injections

This document catalogs every system instruction and prompt injection hardwired into the A.I.M. OS. Use this map to understand the "Cognitive Persona" of each autonomous script and how they work together to maintain technical integrity.

---

## 1. Reincarnation & Continuity
These prompts manage the transition of "Will" and "State" between agent vessels.

| Script | Injection Point | Persona / Objective |
| :--- | :--- | :--- |
| `src/handoff_pulse_generator.py` | `generate_reincarnation_gameplan` | **The [Reincarnation](Reincarnation-Map) Strategist:** Analyzes the full session history tail to distill the project's heartbeat and draft rigid battle plans. |
| `src/handoff_pulse_generator.py` | `generate_handoff_pulse` | **The Continuity Engine:** Surgical technical scribe that identifies the "Project Edge" (What's finished, broken, or next). |
| `scripts/aim_reincarnate.py` | `wake_up_prompt` | **The Wake-up Mandate:** The first thing a new agent hears. Forces the reading of `GEMINI.md`, `HANDOFF.md`, and the `REINCARNATION_GAMEPLAN.md`. |

---

## 2. The Persistent LLM Wiki Pipeline
These prompts govern the refinement of raw session data into Durable Lore.

| Component | Script/File | Persona / Objective |
| :--- | :--- | :--- |
| **Signal Extraction** | `hooks/session_summarizer.py` | **The Subconscious Scribe:** Analyzes raw session transcripts to extract a concise "Signal Skeleton" (core takeaways) and drops it into `wiki/_ingest/`. |
| **Wiki Synthesis** | `wiki/WIKI_SCHEMA.md` | **The Subconscious Wiki Daemon:** Runs in the background, reads the ingested skeletons, and seamlessly weaves the new architectural lore into the native Markdown wiki files. |

---

## 3. Executive Guardrails (Anti-Drift)
These prompts are injected *live* during a session to prevent behavioral degradation.

| Hook | Script | Objective |
| :--- | :--- | :--- |
| **Mantra** | `hooks/cognitive_mantra.py` | **The Mantra Protocol:** Forcefully halts execution after 50 actions and requires the agent to recite its full `GEMINI.md` system instructions to clear "Lost in the Middle" decay. |
| **Whisper** | `hooks/cognitive_mantra.py` | **The Subconscious Whisper:** Injects a silent reminder of mandates every 25 actions to nudge the agent back toward GitOps and TDD. |

---

## 4. Specialized Matrix Operations
Temporary personas used for focused sub-tasks.

| Script | Persona / Objective |
| :--- | :--- |
| `scripts/aim_delegate.py` | **The Specialized Sub-Agent:** A zero-filler analyst that provides binary or short-string answers for massive file processing. |
| `hooks/session_summarizer.py` | **[The Eureka Protocol](The-Eureka-Protocol):** A hindsight-pruning heuristic that captures the exact moment of technical breakthrough. |

---

## 5. Development Utilities
Prompts used by the operator to setup or maintain the environment.

| Script | Objective |
| :--- | :--- |
| `scripts/aim_init.py` | **The Grok Profiler:** A highly-specific prompt used to scrape and mirror the Operator's personality traits from social history. |

---

## 6. Known Adversarial Prompt Injections (Instruction Drift Vectors)
These are *Operator* inputs or tool outputs that have historically caused the agent to abandon its core constraints (e.g., TDD, GitOps, or Absolute Workspace Isolation).

| Attack Vector / Prompt | The Drift Consequence | Mitigation Strategy |
| :--- | :--- | :--- |
| **"Do it quickly" / "Speed" / "Optimization"** | The agent silences the TDD Prime Directive and skips writing or running tests. | **Issue #150:** Hardcoded an `ANTI-DRIFT MANDATE` into `GEMINI.md` explicitly stating that TDD cannot be bypassed for speed. |
| **Missing / Ignored Continuity Files** | The agent hallucinates context and attempts to rewrite existing scripts from scratch because `.gitignore` blocked its tools from reading `HANDOFF.md`. | **Issue #230:** Added an explicit directive to use `run_shell_command` with `cat` to force-read gitignored files. |
| **"Create a new file" / "Dump the output"** | The agent dumps thousands of artifacts into the root directory instead of isolating them, contaminating the Git workspace. | **Issue #204:** Hardcoded the `ABSOLUTE WORKSPACE ISOLATION (THE SANDBOX)` mandate into `GEMINI.md`. |
| **Vague Ticket Titles (No Action Items)** | The agent relies on a massive `FALLBACK_TAIL.md` conversation dump and gets "Lost in the Middle," forgetting the actual bug. | **Issue #201:** Formalized the **Commander's Intent** in the `aim bug` CLI command, forcing the operator to provide explicit Action Items. |