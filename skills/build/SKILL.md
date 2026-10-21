---
name: build
description: Implement the next unchecked task (or a named task) from docs/roadmap.md, with tests, then update the project memory docs.
argument-hint: [optional task name]
disable-model-invocation: true
---

Task to work on: $ARGUMENTS
(If that is empty, take the first unchecked task in docs/roadmap.md.)

1. Read docs/progress.md, docs/architecture.md, and the matching feature
   spec in docs/features/ if one exists. If a project skill in .claude/skills/
   covers this kind of task, follow it.
2. Write a short plan: files to touch, approach, tests. If the task touches
   more than about 3 files or changes the data model, show me the plan and
   wait for approval before coding.
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
   skill and the pattern you saw. Do not create anything yet.
   - If I say yes, follow the make-skill skill.
   - If I say no, add "Declined skill suggestion: <name>" to today's progress.md entry and commit.
8. Suggest `/clear` before the next `/build` to keep context fresh.
