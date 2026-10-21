# Why this exists

If you've built anything with an AI coding agent, you've probably seen this:
day one goes great, and by day three things start slipping. You open a new
session and Claude has no idea what you decided yesterday. It suggests a
different database. It forgets the half-finished feature. Your new idea gets
wedged in somewhere it doesn't fit.

There are big spec-driven frameworks that fix this, but they come with phases,
approval gates and a lot of files. That's a lot of process for a side project
you're building on a Sunday afternoon. This setup is the smallest thing I could
come up with that still keeps Claude on track.

## Your project remembers, so the chat doesn't have to

Every project gets a `docs/` folder of plain markdown files:

- `vision.md`: what you're building, and just as important, what you're not
- `architecture.md`: the stack, how the folders are laid out, the data model
- `roadmap.md`: a checklist of tasks, each small enough for one session
- `decisions.md`: every real choice and why you made it. Nothing gets deleted.
- `progress.md`: a running log of what got done each session
- `features/`: one short spec per feature

Every command reads these before it starts and updates them when it's done.
When you open a new session, a hook prints your latest progress and the roadmap
straight into Claude's context. So it doesn't matter if you cleared the chat,
switched computers or changed models. Claude starts out knowing where you are.

That also means you can `/clear` as often as you like. A fresh session with good
notes does better work than a long one full of old back-and-forth.

## How a project moves

```
your idea  --/plan-idea-->  plan + roadmap  --/build, /build, /build-->  working app
new idea   --/add-feature-->  spec + new tasks  --/build-->  ...
big update finished  --/ship-->  reviewed, tested, deployed
```

`/plan-idea` takes your rambling and asks only the questions that would change
how the thing gets built. Everything else gets a sensible default you can accept
in one word.

`/build` does one task at a time: plan it, write it, test it, log it, commit it.
Small tasks mean small mistakes, and they're easy to undo.

`/add-feature` reads what's already there before it plans anything, so new
features fit into the app instead of fighting it.

`/ship` hands your changes to a reviewer subagent that never saw them being
written, so it actually finds things. Then it runs the tests and build, tags a
version and deploys.

## Why it stays simple

The whole thing is five commands, two subagents and one hook. It's all markdown,
so you can open any file and change it. There's no database or extra tooling.

Nothing runs behind your back. The main commands only fire when you type them.

Planning is light on purpose: a few questions, then you're building. You approve
a plan only when a task is big enough to matter.

It lives in `~/.claude`, so a new project needs no setup at all. And the
subagents use whatever model your session uses, so changing models means
changing one setting.

## Where the quality comes from

You get plans before code, and tests on every task before it counts as done.
A second set of eyes reviews everything before you ship. Past decisions are
written down, so Claude tells you when something conflicts with one instead of
quietly undoing it. And every task starts with a clean context.

## When you might outgrow it

This is built for one person working on personal projects. If yours gets big,
you can run tasks side by side in separate git worktrees with `claude -w <task>`.
If you want to share your version with friends, you can package it as a Claude
Code plugin.
