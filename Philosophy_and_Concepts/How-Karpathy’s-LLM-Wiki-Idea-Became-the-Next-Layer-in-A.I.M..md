# How Karpathy’s LLM Wiki Post Lit a Fire Under A.I.M.

*Written by Brian Vasquez | April 5, 2026 (11:59 AM)*

I saw Andrej Karpathy drop his  [**LLM Wiki**](https://x.com/karpathy/status/2040470801506541998) idea on X and it felt like someone read my mind and then made it 10× cleaner.

He basically said: stop treating LLMs like dumb RAG retrievers that rediscover everything from scratch every time you ask a question. Instead, let the LLM **build and maintain a persistent, interlinked wiki** — a living, compounding knowledge base that sits between you and the raw sources. The LLM does all the boring maintenance (cross-referencing, updating summaries, flagging contradictions, keeping everything consistent) while you just curate sources and steer the thinking.

It’s elegant. It’s exactly the kind of high-signal, compounding memory I’ve been chasing.

But here’s what hit me hardest: **A.I.M. was already 80% of the way there**.

I didn’t need to replace anything in my system. Karpathy’s pattern slotted in perfectly as a **new, natural layer** on top of the memory refinement system I already use every day.

### Why It Maps So Cleanly to A.I.M.

Karpathy’s LLM Wiki has three layers:
- **Raw Sources** (immutable documents)
- **The Wiki** (LLM-owned, interlinked Markdown pages that evolve)
- **The Schema** (rules that turn the LLM into a disciplined maintainer)

Look at what A.I.M. already ships:

- My **Layered Engram Architecture** is literally the same split:  
  - Immutable Base Cartridges (`.engram` files) = Raw Sources + pre-embedded knowledge  
  - Live Delta / Patch Ledger = The evolving wiki layer that gets updated in real time

- **DataJack cartridges** are exactly the pre-baked, shareable wiki pages Karpathy describes. One person embeds the docs once (Python 3.14 + Django + whatever), bakes it, and everyone else can `aim jack-in` and inherit the full knowledge instantly — no re-embedding, no extra cost.

- My **memory refinement system** (cascading distillation, zero-token pruning, Eureka moments) is the perfect engine for the “maintenance” part Karpathy talks about. The LLM doesn’t just summarize — it can surgically prune thrash, distill eureka moments, and keep the wiki clean and high-signal without bloating context.

- **Obsidian Bridge** gives me exactly what he described: LLM on one side, Obsidian open on the other. I watch the graph view grow in real time while the agent updates pages, adds cross-references, and files new insights.

- The **Schema** is already there — my `GEMINI.md` / `CLAUDE.md` files are the exact “discipline layer” he mentions. They tell the agent how to behave as a wiki maintainer instead of a generic chatbot.

So I’m not replacing anything. I’m just **adding the LLM Wiki pattern as the natural next layer** on top of the exoskeleton I already live in.

### What This Looks Like in Practice Now

- I drop a new source (paper, article, chapter, my own notes) into the raw folder.
- I tell the agent: “Ingest this into the wiki.”
- It reads it, updates the relevant entity/concept pages, flags contradictions, adds cross-references, updates `index.md` and `log.md`, and distills the new knowledge using my existing refinement pipeline.
- Because it’s running inside A.I.M., it can also trigger Reincarnation if the session gets long, spawn isolated worktrees for deep analysis, or even bake the new insights as a shareable DataJack for the Sovereign Swarm.

The wiki becomes the **persistent, compounding artifact** Karpathy described, but with all the battle-tested plumbing I already built for long-running autonomous agents.

### The Bigger Picture

This isn’t just a cool new feature — it’s the missing piece that turns A.I.M. from “really good personal memory tool” into something that can actually help build the decentralized knowledge commons I’ve been dreaming about with the Collective Cortex.

Karpathy gave me the clean mental model.  
A.I.M. already had the plumbing.

Now they’re working together.

I’m honestly excited to start using this daily. The vibe coder in me is grinning — this feels like the right evolution.

If you’re already running A.I.M., you can start experimenting with the LLM Wiki pattern right now. Just treat your `docs/wiki/` folder as the wiki layer and let your agent follow Karpathy’s schema ideas inside the existing `GEMINI.md` / `CLAUDE.md` files.

The exoskeleton was always meant for this.