# Architecture Decision Record: The Ghost Orchestrator

**Date:** May 2026
**Status:** Accepted
**Context:** A.I.M. Long-Horizon Benchmarking Infrastructure

## 1. The Problem: The API Wall
When executing massive, marathon benchmarks (like LongMemEval or LoCoMo V2), researchers typically use a standard Python execution loop (`requests.post()`) to feed prompts to an LLM API and await the response. 

However, during our benchmarking of the Google Gemini API (specifically `gemini-3.1-pro-preview` and `gemini-3-flash-preview`), we discovered a catastrophic failure mode that renders standard Python loops useless: **The Silent 429 Hang.**

When the API hits a strict RPM (Requests Per Minute) or TPM (Tokens Per Minute) limit, it often fails to return an explicit 429 error code to the client. Instead, the connection hangs open. The API enters an infinite "Thinking" loop, effectively freezing the benchmark forever. 

During our initial LoCoMo V2 test, the Gemini CLI hung silently for 50 minutes, destroying the test run and forcing a manual restart.

## 2. The Solution: Tmux Injection (The Ghost Orchestrator)
To guarantee 100% benchmark completion without human intervention, A.I.M. abandoned standard API loops and developed the **Ghost Orchestrator** architecture (`runners/locomo_v2_runner.py`).

### A. The Containerized Target
Instead of calling the API directly, the Ghost Orchestrator spawns the target AI agent inside a detached `tmux` terminal session. This isolates the agent from the testing harness.

### B. Polling & Parsing (`ghost_judge.py`)
The Orchestrator operates as an external watchdog. It uses the `tmux capture-pane` command to scrape the raw stdout of the agent's terminal in real-time. It parses this raw text, looking for specific boundary markers (like `[ANSWER]`).

### C. The Escape Key Injection
Because the Orchestrator lives outside the agent's execution thread, it can enforce strict timeouts. 
If the Orchestrator detects that the agent has been "Thinking" for more than 5 minutes (indicating a silent 429 hang), the Orchestrator physically injects an `Escape` key keystroke (`tmux send-keys -t {session} Escape`) directly into the container. 

This mechanical keystroke forcefully breaks the agent out of its coma. The Orchestrator then waits 60 seconds (an API cooldown period) and re-injects the prompt to try again.

## 3. The Result
By wrapping the agent in a `tmux` exoskeleton, the Ghost Orchestrator successfully bypassed the API wall. During the 10.7-hour "Nightmare Run," the Orchestrator detected 31 silent hangs and successfully injected 31 `Escape` commands, allowing the benchmark to complete with a flawless 100% answer rate despite severe upstream server instability.
