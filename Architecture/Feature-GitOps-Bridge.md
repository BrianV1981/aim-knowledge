# Key Feature: The GitOps Bridge

**The Problem:** Autonomous AI agents are notoriously sloppy with version control. They create massive "kitchen sink" commits, bypass issue trackers, and force-push broken code, making it impossible to revert specific regressions.

**The Solution:** The GitOps Bridge. A natively integrated, absolute constraint layer that forces the AI into a strict Issue-Driven Development (IDD) and Semantic Release pipeline.

---

## 1. The Core Philosophy (Atomic Deployments)
A.I.M. enforces a **Continuous Release** methodology. AI agents are strictly forbidden from executing raw `git commit` or `git push` commands. 

If an agent wants to change code, it must follow the 3-step DevOps lifecycle.

## 2. The 3-Step Lifecycle

### Step 1: The Reporter (`aim bug`)
*   **Action:** `aim bug "Title" --context "X" --failure "Y" --intent "Z"`
*   **Mechanism:** A.I.M. forces the Operator or Agent to provide explicit **Commander's Intent** (Context, Failure, Action Items). It then reads the `continuity/FALLBACK_TAIL.md` (the last 10 turns of raw conversation history), wraps it in a collapsible markdown block, and uses the `gh` CLI to instantly create a highly structured bug ticket on the remote GitHub repository. If arguments aren't provided, it prompts interactively.

### Step 2: Absolute Workspace Isolation (`aim fix` / The Factory Floor)
*   **Action:** `aim fix <issue_id>`
*   **Mechanism:** Instantly executes `git worktree add workspace/issue-<id> -b fix/issue-<id>`. 
    *   **The Factory Floor:** Instead of checking out a branch in the root repository, this command leverages **Git Worktrees** to physically spawn a new, isolated directory. The agent is instructed to `cd` into this folder to perform their work.
    *   **Swarm Concurrency:** This allows multiple AI agents (or human operators) to work on different GitHub Issues simultaneously on the exact same machine. Because they are physically isolated, they cannot accidentally `git add .` each other's files, nor will they suffer from `git stash` collisions if interrupted.
    *   **Mail System Synergy:** This worktree protocol perfectly complements the "Chalkboard" decentralized mail system. Just as the mail system uses a standalone folder (`workspace/aim-chalkboard`) to `git pull`, drop a message, and `git push` without modifying the host agent's environment, the Git Worktree protocol treats the main repository solely as the orchestrator. All actual tactical coding occurs safely in isolated sub-folders.

### Step 3: The Semantic Release (`aim push`)
*   **Action:** `aim push "Fix: Patched the auth logic (Closes #4)"`
*   **Mechanism:** This is the crown jewel of the GitOps Bridge. 
    1.  It reads the `Fix:` prefix.
    2.  It reads the `VERSION` file and mathematically calculates a SemVer patch bump (e.g., `v1.2.0` ➔ `v1.2.1`).
    3.  It automatically formats and prepends the new release to the `CHANGELOG.md`.
    4.  It stages, commits, and pushes the code to the isolated branch.

## 3. The Phase Protocol (`aim promote`)
When the feature is complete and the tests are green, the `aim promote` command automates the entire Senior DevOps merge lifecycle and cleans up the Factory Floor:
1. Checks out `main` in the root repository.
2. Creates an immutable archive branch of the *current* state of main (e.g., `archive-phase-24-20260323`).
3. Merges the dev branch into `main`.
4. Pushes the new baseline to GitHub.
5. Deletes the local dev branch.
6. **Worktree Cleanup:** Automatically executes `git worktree remove workspace/issue-<id> --force` to instantly dissolve the temporary physical folder and keep the workspace perfectly clean.

## 4. The Batch Merge Protocol (`aim merge-batch`)
When managing multiple decentralized agents or resolving a batch of minor tickets, the `aim merge-batch` utility allows you to quickly pull and merge all open `fix/issue-*` branches residing on the remote `origin` into `main` sequentially.
*   **Action:** `aim merge-batch [--push]`
*   **Mechanism:** Automates checking out `main`, running `git pull origin main`, iterating over all `origin/fix/issue-*` remote-tracking branches, and running `git merge`. If conflicts arise, the specific branch merge aborts safely while others proceed.

## The Result
Your repository maintains a pristine, granular, automated public ledger. Every single line of code is tied to an issue, and every regression can be instantly reverted without untangling a mega-commit.