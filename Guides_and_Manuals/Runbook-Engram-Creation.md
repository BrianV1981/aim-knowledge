# Runbook: How to Forge an `.parquet` Cartridge

*[The DataJack Protocol](The-DataJack-Protocol) allows you to share pre-vectorized, mathematical semantic memory with other A.I.M. agents without wasting API tokens or GPU cycles. This runbook walks you through the exact process of creating (baking) a new `.parquet` file.*

---

## Step 1: Gather Your Raw Materials
Before you can forge a cartridge, you need raw text data. 

1. Create a temporary ingestion folder inside the `synapse/` directory. 
   *(Note: The `synapse` folder is `.gitignore`'d, meaning anything you drop in here won't accidentally bloat the main A.I.M. repository).*
   
   ```bash
   mkdir -p synapse/my-new-plugin
   ```

2. Download your target documentation. The [DataJack Foundry](The-DataJack-Protocol) automatically parses the following file types:
   *   `.md` / `.markdown` (Standard Docs)
   *   `.txt` (Raw Text / Standard Python Docs)
   *   `.rst` (reStructuredText / Sphinx Docs)
   *   `.py`, `.ts`, `.rs`, `.js` (Raw source code files)

3. Drop all the files into `synapse/my-new-plugin/`.

## Step 2: The "Factory Floor" Protocol (`aim bake`)
You do not need to pollute your active `memory_lance` to create a new cartridge. Because A.I.M. 5.21 uses native Apache Arrow, it doesn't even need to spin up a temporary database to compile knowledge.

Run the `aim bake` command. It requires two arguments: the target directory, and the output filename.

```bash
aim bake synapse/my-new-plugin my_custom.parquet
```

### What Happens in the Background:
1. **Chunking:** It reads all your raw files and chunks them into semantic blocks using the Length-Constrained Accumulator.
2. **Embedding:** It spins up your embedding engine (defaulting to the free local Ollama/Nomic model) and mathematically embeds every chunk into a 768-dimensional float array. To take the cartridge's accuracy to a whole new level, swap your configuration to use [Gemini Embedding 2](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-embedding-2/) before baking.
3. **PyArrow Compilation:** Instead of doing complex database inserts, it simply structures the data in a Python list and uses the `pyarrow` library to strictly define a native columnar schema (`pa.schema`).
4. **Direct Write:** It converts the list into a PyArrow Table and writes the `.parquet` file directly to disk natively. Zero SQLite, zero ZIP wrappers, and zero cross-contamination with your active workflow.

## Step 3: Test Your Cartridge
Your new `my_custom.parquet` file is now sitting in the root of your A.I.M. workspace! 

Before you share it, test it locally to ensure it works. 

1. Move the new cartridge into the dedicated `engrams/` directory:
   ```bash
   mv my_custom.parquet engrams/
   ```
2. Jack it into your active subconscious:
   ```bash
   aim jack-in engrams/my_custom.parquet
   ```
3. Run a test search to verify the hybrid Tantivy FTS and Vector indices caught the data:
   ```bash
   aim search "something from my custom docs"
   ```

## Step 4: Share It with the World
Because `.parquet` files are highly-compressed Parquet files (ranging from 500KB to 30MB), **do not commit them directly to Git.** 

To share them:
1. **GitHub Releases:** If you maintain a public repository, upload the `.parquet` file as an Asset to a GitHub Release.
2. **Direct Transfer:** You can literally email the file, drop it in Slack/Discord, or put it on a USB drive. 
3. **The [DataJack](The-DataJack-Protocol) Torrent Swarm (Phase 38):** You can use `aim jack-in "magnet:?xt=..."` to seed and download cartridges peer-to-peer or use `aim export` to seed your own local cartridges. 

---
*If you are looking for the automated "Self-Farming" engram protocol (where A.I.M. automatically scrapes GitHub issues and generates its own engrams), please refer to `docs/CARTRIDGE_FARMING_ECOSYSTEM.md`.*