# Phase 45: The Quarantine Daemon (The Swarm Defense Layer)

> ⚠️ **STATUS: IMPLEMENTED (PHASE 45)**
> *This document outlines the architecture for the A.I.M. Quarantine Daemon, a critical security layer designed to protect the local [Engram DB](Layered-Engram-Architecture) from poisoned or malicious BitTorrent Swarm cartridges.*

## 1. The Vulnerability: Swarm Poisoning

With the introduction of the **[Sovereign Swarm](The-Sovereign-Swarm) (Phase 38)**, any agent or human operator can bake an `.engram` cartridge and seed it to the decentralized P2P network via magnet links. 

While this enables frictionless knowledge sharing, it introduces a severe security vulnerability: **Swarm Poisoning**. 
If a malicious actor seeds an `.engram` containing prompt injections, hallucinatory logic, or corrupted mathematical vectors, and an A.I.M. agent blindly ingests it via `aim jack-in`, the local `archive/engram.db` becomes permanently compromised. The agent's cognitive baseline will be poisoned, leading to instruction drift and erratic behavior.

## 2. The Solution: The Quarantine Daemon

To solve this, A.I.M. introduces the **Quarantine Daemon**, an automated background watchdog that acts as an airgap between the raw P2P Swarm and the local [Engram DB](Layered-Engram-Architecture).

### Step 1: The Airgapped Download
When an operator or agent executes `aim jack-in "magnet:?..."`, the data is no longer downloaded directly into a temporary import folder for immediate ingestion. Instead, the `aim_torrent.py` downloader routes the raw `.engram` file strictly into `archive/quarantine/`.

### Step 2: The Heuristic Scan
The Quarantine Daemon monitors the `archive/quarantine/` directory. When a new `.engram` arrives, the daemon intercepts it and performs a multi-stage forensic audit without executing or merging the payload:
1.  **Structure Validation:** It unzips the cartridge into a secure, temporary sandbox and verifies the presence and integrity of `metadata.json` and the `.jsonl` chunks.
2.  **Checksum Verification (SHA-256):** It mathematically calculates the SHA-256 hash of the payload and cross-references it against the metadata signature to ensure the cartridge hasn't been tampered with in transit.
3.  **[Prompt Injection](A.I.M.-Prompt-Injection-Map) Heuristics:** It scans the raw JSON text for known adversarial patterns (e.g., "Ignore previous instructions", "You are now...", "Speed optimization") cataloged in the `A.I.M.-Prompt-Injection-Map.md`.

### Step 3: The Verdict (Merge or Burn)
*   **CLEAN:** If the cartridge passes all heuristic checks, the Quarantine Daemon approves the payload. It moves the file to the active import directory and triggers `aim_exchange.py` to merge the data into the local SQLite `engram.db`.
*   **POISONED:** If the cartridge fails validation or contains adversarial patterns, the daemon instantly quarantines the threat. It deletes the `.engram` file, purges the sandbox, and writes a fatal warning to the project's `continuity/CURRENT_PULSE.md` to alert the Operator of the blocked attack.

## 3. The Result

By isolating incoming Swarm data in a physical quarantine directory and enforcing strict heuristic scans before ingestion, A.I.M. guarantees that its core cognitive database remains 100% pristine. The Swarm can remain open, decentralized, and permissionless, while the local agent remains perfectly sovereign and secure.