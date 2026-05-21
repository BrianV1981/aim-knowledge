# Architectural Design: The DataJack Protocol
**Status:** Live — v1.7.0
**Goal:** Implement a zero-compute, instant-knowledge transfer system for A.I.M., allowing operators to share massive, pre-embedded datasets (Brain Plugins) without incurring API or CPU token taxes.

---

## 1. The Core Philosophy ("I Know Kung Fu")
While standard CLIs have "Skills" (which are executable actions/tools), A.I.M. features **The DataJack Protocol** (which is instant memory). 

This is a direct nod to the concept of the *Construct* training programs. Instead of asking the AI to read 10,000 pages of Python documentation and waiting hours for it to generate vector embeddings, an operator can simply download a pre-compiled `.parquet` cartridge. 

Once "jacked in," the AI instantly possesses flawless semantic recall of that subject matter.

```text
> aim jack-in python314.parquet
--- A.I.M. DATAJACK: IMPORT (Native ROM) ---
[INFO] Mounting Parquet Cartridge: python314.parquet
[SUCCESS] Native ROM Cartridge mounted successfully at /archive/cartridges/python314.parquet.
```

## 2. The Mechanics of an `.parquet` Cartridge
A `.parquet` file natively contains:
1. **Embedded Metadata:** Defines the contents (e.g., "Python 3.14 Standard Library").
2. **Arrow Schema:** The raw text *and* the pre-calculated 768-dim float arrays (Nomic Embeddings), optimized for native LanceDB zero-copy reads.

## 3. The Workflow

### Extracting a Cartridge (The Operator)
If you have a directory of raw documentation (e.g., `synapse/react-docs`), you can compile it into a vectorized cartridge:
```bash
aim bake synapse/react-docs react19.parquet
```
*Behind the scenes:* A.I.M. parses the markdown, generates high-dimensional vectors (Nomic math) locally, exports them natively to Parquet files, calculates a strict SHA-256 payload checksum, and writes them directly to the highly-compressed `.parquet` cartridge.

You can then seed this cartridge directly to [the Sovereign Swarm](The-Sovereign-Swarm) P2P network:
```bash
aim export react19.parquet
```

### Jacking In (The Receiver)
When another developer wants to ingest your `react19.parquet` file, they use the magnet link (or the local file) and run:
```bash
aim jack-in "magnet:?xt=urn:btih:..."
```

*Behind the scenes:* 
1. A.I.M. downloads the payload into the airgapped `archive/quarantine/` folder.
2. The **[Quarantine Daemon](Feature-Quarantine-Daemon)** inspects the cartridge metadata, validates the SHA-256 signature, and heuristically scans for prompt injections.
3. If clean, LanceDB mounts the Parquet file directly using **zero-copy reads** (The ROM vs RAM Architecture). The local session database acts as RAM, while the Cartridge acts as Read-Only Memory (ROM).
4. The transfer takes seconds. Zero embedding API calls are made. 

## 4. The Value Proposition
This completely changes how AI knowledge is distributed. Instead of sharing massive raw text files and forcing every developer on Earth to waste GPU cycles embedding them, one person embeds the knowledge once, compiles the `.parquet` cartridge, and shares the mathematical "memories" infinitely.