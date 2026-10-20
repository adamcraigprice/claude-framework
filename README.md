# Adam's Claude Code framework

Personal, project-agnostic setup for building apps from scratch with Claude Code.
Lives in ~/.claude so it works in every project.

## Commands
- `/plan-idea <brainstorm>` - new app: vision, architecture, roadmap (sets up docs/ if missing)
- `/build [task]` - build the next roadmap task with tests, then update docs
- `/add-feature <brainstorm>` - spec a new feature against what already exists
- `/ship` - review, test, tag and deploy after a major update
- `/project-init` - just scaffold CLAUDE.md + docs/ in an existing repo

## Subagents
- `code-reviewer` - fresh-eyes review, read only
- `test-runner` - runs tests/build, diagnoses failures

## Memory
Each project keeps its memory in `docs/` (vision, architecture, roadmap,
decisions, progress, features/). A SessionStart hook in settings.json prints
progress + roadmap into every new session.

## Model
`model` in settings.json. Switch fable -> opus when credits run out
(`/model opus`). Subagents use `model: inherit`, so they follow automatically.

## Sync
This folder is a git repo that only tracks settings.json, skills/, agents/
and this README (see .gitignore). On another machine: clone into ~/.claude.
