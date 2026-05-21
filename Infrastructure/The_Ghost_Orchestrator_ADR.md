# Architecture Decision Record: The Ghost Orchestrator

**Date:** May 2026

## Context
High-horizon LLM benchmarking (e.g., 50+ turn debugging sessions) is fundamentally broken by standard API timeout patterns. Naive `requests.post()` loops inevitably fail due to silent 429 timeouts and model "Thinking" loops.

## The Ghost Solution
A.I.M. bypasses the API wall by utilizing `ghost_runner` and `ghost_judge` as detached daemons. 
1. **`tmux` Injection:** The orchestrator spawns an isolated `tmux` session, gaining physical control over the TTY. 
2. **Key Injection:** Instead of relying on API libraries, it injects keyboard events to trigger terminal input/output, allowing it to "Escape" infinite loops and force-reset stalled models.
3. **Epistemic Judge:** The evaluator runs in an isolated vessel, ensuring the runner process cannot contaminate the evaluation logs.
