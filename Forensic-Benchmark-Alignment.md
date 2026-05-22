# Forensic Benchmark Alignment

This document outlines the evaluation pipelines and ghost judge logic for the LoCoMo and other benchmarks.

## Dataset Purification
### LoCoMo-V2 Dataset
- **Evidence Tags Removed:** Evidence tags were removed from the `locomo-v2` dataset to streamline the purification process.
- **RAG 5.21 Schema Alignment:** The data is aligned with the new RAG 5.21 schema, which uses explicit, non-tagged metadata fields embedded directly within the Parquet cartridges for cleaner hybrid retrieval.
- **Epistemic Honesty (IDK Acceptable):** Appended `(IDK acceptable)` or `(alternative answers accepted)` to subjective/speculative questions across all six benchmark datasets. This prevents literal-match judges from penalizing honest "I don't know" responses where Ground Truth is inherently unknowable or stated as uncertain (Issue #23).