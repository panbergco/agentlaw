# Stop Micromanaging Your AI Agent — Write a Law Instead

> I replaced a 5,000-token CLAUDE.md with 28 lines that actually work. Here's what I learned about plan mode, memory, and why delegation beats control.

---

Most CLAUDE.md files I've seen are micromanagement dressed up as engineering. Hundreds of lines dictating which files to read, which commands to run, what order to do things in. Some hit 5,000 tokens before the agent starts thinking about your actual task.

Every token spent on instructions is a token the agent can't spend on your work. And the more rigid the instructions, the more brittle the agent becomes.

I spent a week building something different: a 28-line operating law for autonomous agents. Here's what I discovered.

## The plan mode trap

I asked the agent to build a full-stack app. Set it loose with `--dangerously-skip-permissions` and waited. 34 minutes later: zero files created.

The agent had entered plan mode — voluntarily switching itself to read-only. It spawned three research subagents, produced excellent analysis, then sat there. Planning. Unable to write a single file.

Turns out plan mode isn't a hard system constraint. It's a prompt injection — a system reminder prepended to every message saying "you MUST NOT make any edits." The write tools are still there. The agent just *chooses* not to use them.

The fix was one line:

```
Never use EnterPlanMode. Execute tasks directly.
```

`EnterPlanMode` isn't even a real tool name. But the agent understood the intent and stayed in execution mode.

## The memory problem

Agents have no memory between sessions. Every new session starts fresh — no idea what was decided or what's half-built. Anthropic's own research confirms this as a core failure mode.

Two more lines fixed it:

```
You have no lasting memory between sessions. Use .memory-bank in project root for persistence.

Read .memory-bank at start, write to it throughout. Goal, success measures, plan, and progress
must exist there before and during execution.
```

The agent now knows it has no memory, knows where to persist, and knows *what* to persist. How it organises those files is its decision. This is a deliberate departure from memory bank patterns like those in Cline and Roo Code, which prescribe exact file structures, naming conventions, and update protocols. I tell the agent what to remember, not how to store it. A capable model can structure its own memory.

## Before and after

Same task, same model, same permissions. Only the CLAUDE.md changed.

**Before:** 34 minutes in plan mode. Zero files. Stuck in read-only research.

**After:** Read `.memory-bank`, found clean slate, wrote goal and success measures, wrote architecture plan, immediately started building — database, API adapters, routes, server, full frontend. All within minutes.

## Delegation, not control

The deeper shift isn't about CLAUDE.md syntax. It's about how we work with agents.

We carry habits from tools that needed precise instructions. Compilers need exact syntax. Pipelines need exact step sequences. We learned that computers do exactly what you tell them, nothing more.

Frontier models reason. They infer. They make judgment calls. The more you constrain their judgment with rigid instructions, the worse those calls become — you've filled their context with rules instead of understanding.

The shift is from control to delegation. From "do steps 1 through 47" to "here's what success looks like, here are the boundaries, go."

If you have to specify everything, you haven't delegated. You've written a very slow script.

## The full law

The [Autonomous Agent Law](CLAUDE.md) is 28 lines. Fork it, test it, see what changes.

The agent is capable. Stop writing it a checklist. Write it a constitution.

## License

MIT
