---
name: project-init
description: Scaffold the project memory system (CLAUDE.md and docs/) in the current repo. Use at the start of a new project, or when CLAUDE.md or docs/ is missing.
---

Set up this repo's memory system. Never overwrite a file that already
exists; only create the missing pieces, and tell me which ones you skipped.

1. If this folder is not a git repo, run `git init`.
2. Create `docs/` with these files. Each starts with a `# Title` heading and
   a one-line italic description of what belongs in it:
   - `vision.md` - what the app is, who it's for, core features, out of scope
   - `architecture.md` - stack, folder layout, data model, key patterns
   - `roadmap.md` - milestones with checkbox tasks, each small enough for one session
   - `decisions.md` - dated log of choices and the reason for each (append only)
   - `progress.md` - dated session log: what changed, files touched, what's unfinished
   - `features/.gitkeep` - one spec file per feature goes in this folder
3. Create `CLAUDE.md` at the repo root with exactly this content:

```markdown
# Project rules

## Memory
This project keeps its memory in docs/. Before planning or coding, read
docs/progress.md, docs/roadmap.md and docs/architecture.md. Read
docs/decisions.md before changing anything architectural. Never contradict
a logged decision without flagging it to me first.

## After every task
- Tick the task in docs/roadmap.md
- Add a short dated entry to docs/progress.md (what changed, files touched, anything left unfinished)
- If you made an architectural or library choice, log it in docs/decisions.md with the reason
- Update docs/architecture.md if structure or data model changed

## How to work
- Plan before coding on anything bigger than a small fix
- Write or update tests alongside code; run them before saying a task is done
- Small commits with clear messages, one task per commit
- Ask me when something is ambiguous rather than guessing on product decisions

## Commands
(fill in once the stack exists: dev, test, lint, build, deploy)
```

4. Make sure `.gitignore` contains `.claude/worktrees/`, `.claude/settings.local.json`
   and `.env*` (create the file if needed, append only missing lines).
5. Commit with the message "Initialize project memory".
6. Tell me it's ready in one line, and that the next step is `/plan-idea`.
