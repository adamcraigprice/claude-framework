---
name: auto-continue
description: Turn auto-continue (each /build chaining into the next task in a fresh subagent until a decision needs me or the milestone ends) on or off, for this project or globally, or show the current setting.
argument-hint: on | off | default | status   [global]
disable-model-invocation: true
---

Arguments: $ARGUMENTS

The setting is a single line, `- Auto-continue: <value>`, in one of two places:
- Project: the "Framework settings" section of this repo's CLAUDE.md.
  Value is `on`, `off` or `default` (default means "use the global setting").
- Global: `~/.claude/framework-settings.md`. Value is `on` or `off`.

How the effective setting is worked out: if the project says `on` or `off`,
that wins. Otherwise use the global value. If neither exists, it's `off`.

Do exactly one of these, then stop:

- **No arguments or `status`:** read both places and tell me in two lines what
  the project and global values are and which one is in effect.
- **`on`, `off` or `default` without `global`:** set the project value in
  CLAUDE.md. If the "Framework settings" section or the line is missing, add it
  at the end of CLAUDE.md. Commit with "Auto-continue: <value>".
- **`on` or `off` with `global`:** set the value in `~/.claude/framework-settings.md`
  (create the file with that one line if it's missing). Do not commit it
  automatically. Remind me to commit and push `~/.claude` if I want it on my other computers.

Confirm the new effective setting in one line. When it's on, `/build` stops only
for decisions that need me, a skill suggestion, or the end of the milestone (see
the build skill).
