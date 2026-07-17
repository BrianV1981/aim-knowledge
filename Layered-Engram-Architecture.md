# Layered Engram Architecture

This document describes the data persistence and retrieval design.

## RAG 5.21 LanceDB/Parquet Native ROM Architecture
As of Issue #588, the legacy SQLite FTS5, `.engram` format, and overlapping-window chunking have been deprecated and surgically replaced.
The system now uses the RAG 5.21 LanceDB/Parquet Native ROM architecture.

- **Storage Format:** Parquet Native ROM
- **Vector Database & Search:** LanceDB with Tantivy for lexical fallback.
- **Data Ingestion:** Automated detached background ingestion uses speaker-boundary chunking logic (Length-Constrained Accumulator) for flight recorders.
- **Maintenance:** A formal LanceDB compaction routine is integrated into `maintenance.py` to mitigate version bloat.
- **Compilation:** The updated wiki is compiled into the `aim_os.parquet` cartridge using the `aim bake` foundry process.
- **Deployment:** The `aim_os.parquet` cartridge is deployed to `assets/default_engrams/`, and `aim` init scripts are updated for native provisioning.
- **Hygiene:** The legacy `engrams/` directory is deprecated and cleaned up, and technical documentation is consolidated into the `aim_os/` directory (formerly `protocol/`).