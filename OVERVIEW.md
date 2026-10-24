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

Each of those files starts from a template with fixed sections. That sounds like
a small thing, but it matters over months. Claude updates "Data model" or
"Milestone 2" in place instead of tacking notes onto the bottom, so the files
stay readable. Sections like "Out of scope" always get filled in, and every
project you open looks the same.

## It picks up your project's habits

The notes cover what you've built. Project skills cover how you build it.

Every app has a few things you end up doing again and again: another API route,
another screen, another migration. The first time, Claude works out the steps.
By the third time it should just know them, and it should stop making the
mistake you already corrected twice.

So after each `/build`, Claude checks for exactly that. If it just repeated a
procedure, or you've had to correct it on the same thing twice, it asks whether
you want to save the steps as a skill for this project. If you say yes, it
writes a short skill that points at your real code as the example. From then
on, every route or screen follows the same pattern. If you say no, it won't ask
again about that one.

There are a few guardrails so this doesn't turn into a mess. It only asks after
real repetition, never during planning. Facts and one-line rules go in the
notes, not in skills. Nothing gets saved without your yes. `/ship` checks the
skills against your code and flags any that have gone stale. If you'd rather
not be asked at all, `/skill-suggestions off` turns it off for one project or
for everything.

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
Small tasks mean small mistakes, and they're easy to undo. With auto-continue on,
it chains those tasks through a milestone, each in a fresh subagent, and stops
only when a decision really needs you.

`/add-feature` reads what's already there before it plans anything, so new
features fit into the app instead of fighting it.

`/ship` hands your changes to a reviewer subagent that never saw them being
written, so it actually finds things. Then it runs the tests and build, tags a
version and deploys.

## Why it stays simple

The whole thing is seven commands, two subagents, one hook and a few templates. It's all markdown,
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

## Isn't this just plan mode?

Not quite, and you'll still use plan mode with it.

Plan mode plans one task inside one session. Claude reads the code without
touching anything, writes up a plan, and you approve it. That's great for the
task in front of you. But the plan only lives in that conversation. Once you
`/clear` or close the window, it's gone. Next week Claude won't know it existed,
what you decided along the way, or what's still left to do.

This framework plans the whole project and keeps that plan in your repo. The
roadmap says what's done and what's next. The decisions log says why things are
the way they are. The progress log says what happened last time. Every session
starts with that loaded in, so you never have to re-explain your app.

Think of it as two levels. The framework is the project plan. Plan mode, or the
plan step inside `/build`, is the task plan. `/build` reads the project plan,
writes a task plan, asks you to approve it when the task is big enough, then
codes it and updates the project plan when it's done.

| | Plan mode | This framework |
|---|---|---|
| Scope | One task | The whole project, from idea to shipped |
| Lasts | Until the session ends | Forever, in your repo |
| Remembers decisions | No | Yes, in `decisions.md` |
| Knows what's done and what's next | No | Yes, in `roadmap.md` and `progress.md` |
| Works across computers and models | No | Yes, it's just files |
| Handles new ideas months later | You re-explain everything | `/add-feature` reads what exists first |
| Testing, review, deploy | Up to you | Built into `/build` and `/ship` |

## What you actually get out of it

**You stop re-explaining your app.** Open a session a month from now and Claude
already knows the stack, the data model and what you were halfway through.

**Your ideas don't get lost.** A half-baked thought becomes a spec and a few
roadmap tasks in a couple of minutes, even if you won't build it for weeks.

**Claude stops undoing your choices.** Picked SQLite over Postgres for a reason?
It's written down, and Claude will flag it before going against it instead of
quietly swapping it out.

**New features fit.** Because `/add-feature` reads the architecture and past
decisions first, new work follows the patterns you already have instead of
inventing a second way of doing the same thing.

**You only correct Claude once.** When you fix how it does something it keeps
doing, that fix can go into a project skill, and every later attempt follows it.

**Mistakes stay small.** Each `/build` is one task and one commit. If something
goes wrong you roll back one step, not a whole afternoon.

**You get better work for fewer tokens.** Clearing between tasks (or a fresh
subagent per task, with auto-continue) keeps the context short and focused. Claude reads a few short notes instead of dragging
around hours of old conversation, which costs less and gets better results.

**You can pick up anywhere.** Different computer, different model, a few months
off: the notes are in the repo, so you just open it and run `/build`.

**You end up with a paper trail.** The vision, architecture and decision log
read like a design doc. That's handy when you're showing the project to someone,
writing it up for a portfolio, or bringing a friend in to help.

**It isn't tied to Claude.** The notes are plain markdown, so other AI tools, or
just you on your own, can pick up the project from the same files.

## When you might outgrow it

This is built for one person working on personal projects. If yours gets big,
you can run tasks side by side in separate git worktrees with `claude -w <task>`.
If you want to share your version with friends, you can package it as a Claude
Code plugin.
