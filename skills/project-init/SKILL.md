---
name: project-init
description: Scaffold the project memory system (CLAUDE.md and docs/) in the current repo from the framework templates. Use at the start of a new project, or when CLAUDE.md or docs/ is missing.
---

Set up this repo's memory system from the templates in
`~/.claude/skills/project-init/templates/`. Never overwrite a file that
already exists; only create the missing pieces, and tell me which ones you skipped.

1. If this folder is not a git repo, run `git init`.
2. Create `docs/` and `docs/features/`, then copy each template that is missing:
   - `templates/vision.md` -> `docs/vision.md`
   - `templates/architecture.md` -> `docs/architecture.md`
   - `templates/roadmap.md` -> `docs/roadmap.md`
   - `templates/decisions.md` -> `docs/decisions.md`
   - `templates/progress.md` -> `docs/progress.md`
   - `templates/feature.md` -> `docs/features/_template.md`
   - `templates/CLAUDE.md` -> `CLAUDE.md` (repo root)
   Copy them with `cp -n` so nothing existing is overwritten. Keep the
   `<!-- -->` guidance comments; they tell you what each section is for.
3. If CLAUDE.md already existed but has no "Where knowledge goes" or
   "Framework settings" section, append those two sections from the template.
4. Make sure `.gitignore` contains `.claude/worktrees/`, `.claude/settings.local.json`
   and `.env*` (create the file if needed, append only missing lines).
   Do not ignore `.claude/skills/`; project skills are committed.
5. Commit with the message "Initialize project memory".
6. Tell me it's ready in one line, and that the next step is `/plan-idea`.
