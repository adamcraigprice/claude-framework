# Overview: why this framework exists

## The problem
Building a personal app with an AI agent usually goes well for a day and then
drifts. Each new session starts with no memory of what was decided, so Claude
re-litigates the stack, forgets half-built features, and new ideas get bolted on
in ways that fight the existing code. Big frameworks fix this with lots of
ceremony, which is overkill when it's just me building something for fun.

## The goal
Go from a messy brainstorm to a shipped app, and keep adding features for months,
without Claude losing the thread and without me managing a process.

## The core idea: the repo remembers, not the chat
Every project keeps a small set of plain markdown files in `docs/`:

- **vision.md** - what we're building and what we're not
- **architecture.md** - the stack, folder layout and data model
- **roadmap.md** - a checklist of small tasks, one session each
- **decisions.md** - what was decided and why, never deleted
- **progress.md** - a running log of what got done
- **features/** - one spec per feature

The skills read these before working and update them after. A hook prints the
latest progress and roadmap into every new session. So any session, on any
machine, with any model, starts already knowing where things stand. Chat history
becomes disposable, which means I can `/clear` freely and keep context sharp.

## The flow
```
brainstorm --/plan-idea--> vision + roadmap --/build (repeat)--> working app
     new idea --/add-feature--> spec + new roadmap tasks --/build--> ...
                         big update done --/ship--> reviewed, tested, deployed
```

1. **Plan.** `/plan-idea` turns a rambling idea into a plan, asking only the
   questions that change the build and offering defaults for everything else.
2. **Build.** `/build` takes one task, plans it, codes it, tests it, logs it,
   commits it. Small tasks keep quality high and mistakes cheap.
3. **Grow.** `/add-feature` reads what exists before planning anything new, so
   features fit the architecture instead of fighting it.
4. **Ship.** `/ship` gets a reviewer that has never seen the code (a separate
   subagent), runs the tests and build, then tags and deploys.

## How it stays simple
- **Five commands, two helpers, one hook.** That's the whole thing.
- **Plain markdown.** No database, no custom tooling. I can read and edit any of it.
- **Manual triggers.** The main commands only run when I type them, so nothing
  surprising happens in the background.
- **Lightweight planning.** No formal requirements phase or approval gates for
  every step. It asks a few questions, sets defaults, and gets building.
- **One model setting.** Subagents inherit whatever the main session uses, so
  switching from Fable to Opus is a single change.
- **Global, not per project.** It lives in `~/.claude`, so new projects need zero setup.

## Where the quality comes from
- **Plan before code**, and for bigger tasks I approve the plan first.
- **Tests on every task**, run before anything is called done.
- **A separate reviewer** who didn't write the code catches what the author misses.
- **Decisions are logged**, so Claude flags conflicts instead of silently undoing past choices.
- **Fresh context per task**, because a cleared session with good docs beats a long, cluttered one.

## When to reach for more
This is tuned for solo personal projects. If a project gets big enough to need
parallel agents, add `claude -w <task>` worktrees. If I ever want to share it,
it can be packaged as a Claude Code plugin.
