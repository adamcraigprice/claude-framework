---
name: make-skill
description: Create or update a project-specific skill in this repo's .claude/skills/ that captures a repeated multi-step procedure. Use only when the user asks for a project skill, or has just approved one you suggested.
argument-hint: <the procedure to capture, e.g. "how we add API routes">
---

Procedure to capture: $ARGUMENTS
(If empty, use the skill the user just approved in this conversation.)

1. **Check it belongs in a skill.** A fact about the app belongs in
   docs/architecture.md and a one-line rule belongs in the Rules section of
   CLAUDE.md. If it's one of those, put it there instead, tell me, and stop.
2. **Check for overlap.** List `.claude/skills/` in this repo. If an existing
   project skill already covers this, update that one instead of making a new one.
3. **Study the real thing.** Find 1-3 places in the code where this procedure was
   done, and any corrections I gave in this conversation or in docs/progress.md.
4. **Draft the skill** at `.claude/skills/<kebab-name>/SKILL.md`:
   - Frontmatter: `name`, and a `description` that says precisely what it does
     and when to use it (specific enough that it won't fire on unrelated work).
   - Body under ~40 lines: when to use it, the numbered steps, the checks that
     prove it worked (tests to run, what to look at), and gotchas.
   - Point at the example files by path instead of pasting code, so it doesn't go stale.
   - Never touch skills in `~/.claude/skills/`.
   - Claude Code always asks permission before writing inside `.claude/`. If the
     write is blocked, tell me to approve the prompt; don't save it anywhere else.
5. **Approval.** If I already said yes to this exact skill in this conversation,
   save it and show me the final text. Otherwise show me the draft and wait for an ok.
6. **Record it.** Add or update a one-line entry in the "Project skills" section
   of docs/architecture.md, add a progress.md entry, and commit with
   "Add project skill: <name>". Tell me it will be available from the next session.
