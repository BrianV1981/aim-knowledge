# Architecture Decision Record: The Ghost Orchestrator

**Date:** May 2026
**Status:** Integrated
**Scope:** Automated Benchmarking & Evaluation

## 1. The Problem: The API Wall
Standard `requests.post()` loops fail against modern LLM API rate limits. During a 63MB Gemini CLI stress test, we observed 31 occurrences of silent "429 No Capacity" hangs, where the agent entered an infinite "Thinking" state.

## 2. The Solution: Tmux-Backed Injection
The "Ghost Orchestrator" (`ghost_runner` / `ghost_judge`) bypasses the application layer by taking control of a `tmux` container.
- **Direct Control:** Instead of network requests, it performs raw stdin key injections (e.g., triggering `Escape` to break out of hangs).
- **Epistemic Isolation:** The Judge agent runs in a distinct `tmux` session, ensuring zero environmental contamination between the runner and the evaluator.

## 3. Impact
This allows A.I.M. to force-evaluate massive datasets while maintaining 99% uptime, even when the underlying LLM provider is struggling with load.
