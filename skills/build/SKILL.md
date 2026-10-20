---
name: build
description: Implement the next unchecked task (or a named task) from docs/roadmap.md, with tests, then update the project memory docs.
argument-hint: [optional task name]
disable-model-invocation: true
---

Task to work on: $ARGUMENTS
(If that is empty, take the first unchecked task in docs/roadmap.md.)

1. Read docs/progress.md, docs/architecture.md, and the matching feature
   spec in docs/features/ if one exists.
2. Write a short plan: files to touch, approach, tests. If the task touches
   more than about 3 files or changes the data model, show me the plan and
   wait for approval before coding.
3. Implement it, following the patterns in architecture.md.
4. Write or update tests and run them. Fix failures before moving on. If
   there is a UI change, run the dev server and check it actually works.
5. Do the "After every task" steps from CLAUDE.md, then commit.
6. Tell me in 2-3 lines what changed and what the next task is. Suggest
   `/clear` before the next `/build` to keep context fresh.
