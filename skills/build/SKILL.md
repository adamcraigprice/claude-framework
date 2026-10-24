---
name: build
description: Implement the next unchecked task (or a named task) from docs/roadmap.md, with tests, then update the project memory docs.
argument-hint: [optional task name]
disable-model-invocation: true
---

Task to work on: $ARGUMENTS
(If that is empty, take the first unchecked task in docs/roadmap.md.)

**Auto-continue setting.** Work it out first: the "Auto-continue" line in
CLAUDE.md's Framework settings wins if it says `on` or `off`; otherwise use
`~/.claude/framework-settings.md`; if neither exists, it's `off`.

**Decisions that need me.** Whatever the setting, stop and ask me (before
writing code, if you can see it coming) when the task needs any of these:
- a product decision the docs don't answer, or anything ambiguous;
- going against a logged decision in docs/decisions.md;
- something only I can provide (keys, sample files, accounts, a manual step);
- a check that still fails after you've tried to fix it, or a merge that's refused;
- anything the project's CLAUDE.md says needs my approval (beyond plan approval).
An architectural or library choice that fits the docs is not one of these:
make it, log it in docs/decisions.md and say so in your summary.

1. Read docs/progress.md, docs/architecture.md, and the matching feature
   spec in docs/features/ if one exists. If a project skill in .claude/skills/
   covers this kind of task, follow it.
2. Write a short plan: files to touch, approach, tests.
   - Auto-continue `off`: if the task touches more than about 3 files or
     changes the data model, show me the plan and wait for approval before coding.
   - Auto-continue `on`: don't wait for plan approval. Show the plan only if it
     raises one of the decisions above, and then wait for my answer. Otherwise
     go ahead, and put the plan's key choices in the task's progress.md entry.
3. Implement it, following the patterns in architecture.md.
4. Write or update tests and run them. Fix failures before moving on. If
   there is a UI change, run the dev server and check it actually works.
5. Do the "After every task" steps from CLAUDE.md, then commit.
6. Tell me in 2-3 lines what changed and what the next task is.
7. **Skill check.** Work out the skill-suggestion setting: the "Skill suggestions"
   line in CLAUDE.md's Framework settings wins if it says `on` or `off`;
   otherwise use `~/.claude/framework-settings.md`; if neither exists, it's `on`.
   If it's `off`, skip this step silently. If it's `on`, suggest a project skill
   only when at least one of these is true:
   - This task repeated a procedure already done at least once before
     (check docs/progress.md and `git log --oneline -30`), and the roadmap shows more coming.
   - I corrected you on the same procedure twice (in this conversation or in progress.md).
   - It's a long, easy-to-get-wrong procedure (deploy, migration) that will clearly recur.
   Skip it if it's really a fact (architecture.md) or a one-line rule (CLAUDE.md
   Rules), if a project skill already covers it, or if progress.md shows I
   declined this one before. Otherwise ask in one or two lines, naming the
   skill and the pattern you saw. Do not create anything yet. A suggestion is a
   question for me, so it stops auto-continue (step 8).
   - If I say yes, follow the make-skill skill.
   - If I say no, add "Declined skill suggestion: <name>" to today's progress.md entry and commit.
8. **Next task.**
   - Auto-continue `off`: suggest `/clear` before the next `/build` to keep context fresh.
   - Auto-continue `on`: follow "Auto-continue" below.

## Auto-continue

You can't run `/clear` or `/build` yourself, so each next task goes to a fresh
subagent instead: it starts with empty context, like `/clear`, and does what
`/build` does.

**If you are a subagent running one chained task** (your prompt says so):
do that one task by the steps above, as if Auto-continue is `on`, and never
start another task or subagent. You can't ask me questions, so when you hit a
decision that needs me, stop before writing code (or leave the work
uncommitted and say what state it's in) and return the question. In step 7,
don't ask: return the suggestion. End with a short report:
- `Status: done`, `needs-decision` or `blocked`;
- what changed (2-3 lines), the commit, and whether it merged if the project
  merges after each task;
- the question, the skill suggestion or the blocker, if any.

**If you are the main session**, keep going while all of these hold:
- the task you just did (or the last subagent) finished with `Status: done`;
- nothing is waiting on me: no question, skill suggestion or blocker;
- `git status` is clean;
- the next unchecked task in docs/roadmap.md is in the same milestone as the one
  just done. When the milestone is finished, stop and tell me it's done.

Then start the next task with the Agent tool (general-purpose, in the
foreground). Give it this prompt, with the task filled in:

> You are running one chained /build task for this project. Read
> ~/.claude/skills/build/SKILL.md and follow it for this task: "<task as
> written in docs/roadmap.md>". Treat Auto-continue as `on`, and follow the
> part for a subagent running one chained task. Follow the project's
> CLAUDE.md (read it first) including its "After every task" steps.

When it returns, tell me its 2-3 line summary, then check the conditions again.
If it stopped on a question, a skill suggestion or a blocker, show it to me and
stop. Once I answer, give the same task to a fresh subagent with my answer added
to the prompt (or follow make-skill for a skill I approved), then carry on with
the chain.

When the chain stops for any reason, say why in one line and suggest `/clear`
before the next `/build`.
