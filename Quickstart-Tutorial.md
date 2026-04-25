# Day 1 Quickstart: Your First A.I.M. Feature

Welcome to A.I.M.! This guide is designed to get you up and running with zero friction. We will walk through exactly how to create a feature, use the AI agent safely, and deploy your code.

**The Golden Rule of A.I.M.:** *You never code directly in the main directory.* Everything happens in an isolated "Factory Floor" Worktree.

---

## Step 1: Tell A.I.M. What You Want to Do
You've just installed A.I.M. and you're sitting in your project directory (e.g., `~/aim`). You want to add a new "Hello World" Python script.

Instead of opening your editor and creating a file, you must first create an **Issue Ticket**. This anchors your agent to a specific goal.

Run this command in your terminal:
```bash
aim bug-operator "Feature: Add a Hello World script"
```
The terminal will pause and ask you for the **Commander's Intent**. This is where you give explicit instructions so the AI agent doesn't have to guess.

**You will see:**
```text
1. The Context (What were you trying to do?):
```
*You type:* `I am trying to learn how A.I.M. works by creating a simple script.`

```text
2. The Failure/Goal (What went wrong / What needs to be built?):
```
*You type:* `We need a simple script named hello.py that prints 'Hello, Swarm!'.`

```text
3. Action Items (What are the precise steps to fix this?):
```
*You type:* `1. Create hello.py. 2. Make it print 'Hello, Swarm!'. 3. Test it.`

A.I.M. will instantly create a GitHub Issue (e.g., **Issue #1**).

---

## Step 2: Enter the Factory Floor (Isolation)
Now that you have Issue #1, you need an isolated workspace so you don't break your main project.

Run:
```bash
aim fix 1
```

A.I.M. will create a brand new, sterile folder just for this issue and tell you to enter it:
```bash
cd workspace/issue-1
```
*You are now on the Factory Floor. You can safely let your AI agent run wild in here. It cannot accidentally delete your main project files.*

---

## Step 3: Unleash the Agent
Now that you are safely inside `workspace/issue-1`, it's time to wake up your AI agent.

Run your standard Gemini CLI or Claude command and ask it to fulfill the ticket:
```bash
gemini "Please read the open issue ticket in continuity/ISSUE_TRACKER.md and execute the Action Items."
```
*(If you are the human operator, you can just open your IDE to `workspace/issue-1` and write the code yourself!)*

Let's assume the agent creates `hello.py` and runs `python3 hello.py` to ensure it works. 
**The tests are green. The feature is done.**

---

## Step 4: The Semantic Release (Deploying)
You are still inside `workspace/issue-1`. It's time to deploy your perfect `hello.py` script.

Instead of `git add .` and `git commit`, A.I.M. automates the deployment securely.

Run:
```bash
aim push "Feature: Implement Hello World script (Closes #1)"
```

A.I.M. reads the word `Feature:`, automatically bumps your project version (e.g., `v1.0.0` -> `v1.1.0`), updates the `CHANGELOG.md`, and pushes your branch to GitHub.

---

## Step 5: Clean Up (Promote to Main)
Your code is safe on GitHub, but your main project folder doesn't have it yet! You need to "Promote" it.

Run:
```bash
aim promote
```

**Watch the magic happen:**
1. A.I.M. automatically merges your new feature into the `main` branch.
2. It pushes the final `main` branch to GitHub.
3. It completely deletes the `workspace/issue-1` folder, leaving your computer perfectly clean.

You are now back in your root directory, your feature is live, and your repository has a flawless Git history.

---

## What's Next?
Now that you know how to build code safely, you might notice your AI agent starting to "forget" things after 50+ turns of chatting. 

When that happens, you need to teleport its brain into a fresh body.
**Read:** [[Context Management ([Reincarnation](Reincarnation-Map))|User-Guide#2-context-management-[reincarnation](Reincarnation-Map)]]

If you want to download expert knowledge from the community into your AI's brain instantly:
**Read:** [[[The Sovereign Swarm](The-Sovereign-Swarm) & [DataJack](The-DataJack-Protocol)|User-Guide#3-knowledge-sharing-the-sovereign-swarm--[datajack](The-DataJack-Protocol)]]