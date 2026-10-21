---
name: ship
description: Review, test and deploy after a major update.
disable-model-invocation: true
---

1. Find the last release tag (`git describe --tags --abbrev=0`). If there
   is none, use the last 20 commits.
2. Use the code-reviewer subagent on all changes since then. Show me its
   findings grouped by severity.
3. After I confirm, fix anything rated critical or high.
4. Use the test-runner subagent to run the full test suite and a
   production build. Fix and re-run until both pass.
5. Update README.md if setup or usage changed.
5b. Check each project skill in .claude/skills/ against the current code: do
   its steps and example file paths still match how things are done? List any
   that are stale and offer to fix or delete each one. Change nothing until I
   say which. Keep the "Project skills" section of architecture.md in sync.
6. Add a progress.md entry summarising the release, commit, and tag it
   with the next version (vX.Y).
7. Push, then deploy using the deploy command in CLAUDE.md. Ask me before
   pushing and before deploying. If there is no deploy command yet,
   suggest a host suited to the stack and ask me which one to set up.
