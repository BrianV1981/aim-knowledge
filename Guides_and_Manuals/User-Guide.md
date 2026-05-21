

# A.I.M. User Guide & Daily Operations

Welcome to the definitive guide on how to actually use the A.I.M. exoskeleton in your daily development workflow. This document provides step-by-step instructions for utilizing A.I.M.'s core features: GitOps Worktrees, [Reincarnation](Reincarnation-Map), [the Sovereign Swarm](The-Sovereign-Swarm), and the [Obsidian Bridge](Obsidian-Bridge-Architecture).

---

## ⚠️ Important First-Time Setup Disclaimer

**Before you start working with A.I.M.**, please read this:

A.I.M. can sometimes leave behind **loose files or stale project artifacts** from previous runs, experiments, or incomplete setups. These can cause unexpected behavior or conflicts if not cleaned up.

**If you are new to the project or want the cleanest possible experience**, we strongly recommend doing a fresh initialization.

### Recommended Setup Steps:

1. Navigate to your main A.I.M. workspace folder (usually):
   ```bash
   cd ~/aim
   ```

2. Run the initialization command:
   ```bash
   aim init
   ```

The `aim init` command will walk you through the complete onboarding process. It lets you:
- Clear any previous project state
- Create a fresh set of guardrails and rules
- Set your Operator name
- Complete the entire configuration

This is the recommended way to start clean and avoid any leftover files.

If you are already very familiar with the project and have a working setup, you can usually skip this step.

---

## 1. The GitOps Bridge & The Factory Floor

A.I.M. strictly prohibits you and your AI agents from coding directly on the `main` branch or dumping files loosely into your root directory. All work must be tied to an issue and performed in an isolated Git Worktree.

### Step 1: Report the Issue
Before writing code, you must declare your intent. Use the `aim bug` command to create a structured ticket on GitHub.
```bash
aim bug "Title of your bug or feature" \
  --context "What you are trying to achieve" \
  --failure "What is currently broken or missing" \
  --intent "The exact step-by-step plan to fix it"
```
*Note: If you just type `aim bug "Title"`, the CLI will interactively prompt you for the Commander's Intent.*

### Step 2: Spawn the Factory Floor
Once the issue is created (e.g., Issue #42), you isolate the workspace.
```bash
aim fix 42
```
This does **not** just change your branch. It physically spawns a new, isolated directory using **Git Worktrees** located at `workspace/issue-42`. 

### Step 3: Do the Work
Change directory into your new isolated workspace:
```bash
cd workspace/issue-42
```
You can now run your AI agent, write code, and run tests (`pytest`). Because you are in a Worktree, you can have 5 different agents working on 5 different issues in 5 different folders simultaneously without any file lock or `git stash` collisions.

### Step 4: The Semantic Release
When the tests are green and the work is complete, deploy the fix atomically:
```bash
aim push "Fix: Resolved the database query bug (Closes #42)"
```
This automatically calculates the semantic version bump, updates the `CHANGELOG.md`, and pushes the branch to GitHub.

### Step 5: Phase Promotion
To merge your isolated feature into the main repository and clean up your workspace:
```bash
aim promote
```
This merges the branch into `main` and **instantly deletes the `workspace/issue-42` folder**, leaving your root directory perfectly clean.

---

## 2. Context Management (Reincarnation)

When your AI agent has been coding for 50+ turns, its context window will become bloated and it will start to hallucinate or forget its core mandates. You must Reincarnate it.

### Step 1: Trigger Reincarnation
From your active terminal session, tell your agent to trigger [reincarnation](Reincarnation-Map), or run the command manually:
```bash
aim reincarnate "Your Commander's Intent for the next agent goes here."
```

### Step 2: The Handoff
A.I.M. will mechanically distill the previous session, generate a `CURRENT_PULSE.md` and a `REINCARNATION_GAMEPLAN.md`, and spawn a brand-new, clean-context agent in a detached `tmux` session.

The CLI will output a specific "Wake up" prompt. **Copy this prompt.**

### Step 3: Teleport
Attach to the new `tmux` session:
```bash
tmux attach-session -t aim_reincarnation_<timestamp>
```
Paste the "Wake up" prompt you copied. The new agent will read its Gameplan, pulse, and issue tracker, and resume coding with maximum velocity and zero context bloat.

---

## 3. Knowledge Sharing (The Sovereign Swarm & DataJack)

A.I.M. allows you to package massive documentation sets into portable `.parquet` cartridges and share them globally via a decentralized BitTorrent swarm.

### Baking a Cartridge
1. Drop raw documentation files (`.md`, `.py`, `.txt`) into a folder like `synapse/my-docs`.
2. Compile them into a vectorized cartridge:
```bash
aim bake synapse/my-docs my_custom_knowledge.parquet
```

### Seeding to the Swarm
If Swarm Peering is enabled in your `aim tui` settings, you can seed your cartridge to the P2P network:
```bash
aim export my_custom_knowledge.parquet
```
The CLI will generate a Magnet Link that you can share with other developers.

### Jacking In (Downloading & Ingesting)
To absorb an engram from the Swarm, use the `jack-in` command with the magnet link:
```bash
aim jack-in "magnet:?xt=urn:btih:..."
```
**The [Quarantine Daemon](Feature-Quarantine-Daemon):** The payload is downloaded to an airgapped `archive/quarantine/` directory. A background watchdog will verify the SHA-256 cryptographic signature and scan the Parquet metadata for adversarial prompt injections. If the cartridge is clean, it is securely mounted as a zero-copy ROM in your `archive/cartridges/` directory.

---

## 4. The Decoupled Brain (Obsidian Bridge)

If you configured an Obsidian Vault in `aim tui`, A.I.M. mirrors your cognitive database (`memory/`, `docs/`, `continuity/`, `core/`) to Obsidian so you can read it beautifully formatted.

### Two-Way Sync
If you manually edit a file inside your Obsidian Vault (for example, updating `docs/ROADMAP.md`), you can force A.I.M. to ingest your human edits back into its active context:
```bash
aim ingest
```
This pulls the newer files from the vault and instantly re-indexes the [Engram DB](Layered-Engram-Architecture) so the AI immediately knows about your changes.

---

## 5. System Maintenance & Configuration

Almost every behavior in A.I.M. is driven by a configurable variable exposed in the TUI (`aim tui`). For a full list of everything you can configure (from LLM providers to Reincarnation thresholds), see the **[Configuration & Variables](Configuration-and-Variables)** guide.

*   **`aim tui`**: Open the configuration cockpit to swap LLM providers, change context routing, and toggle [the Sovereign Swarm](The-Sovereign-Swarm).
*   **`aim update engine`** / **`aim update project`**: Safely pull the latest A.I.M. OS updates from GitHub or your target project code without wiping your local memory database.
*   **`aim status`**: View the current `CURRENT_PULSE.md` to see exactly what the active agent is currently trying to achieve.