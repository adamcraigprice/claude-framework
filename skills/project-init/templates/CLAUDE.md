# Project rules

## Memory
This project keeps its memory in docs/. Before planning or coding, read
docs/progress.md, docs/roadmap.md and docs/architecture.md. Read
docs/decisions.md before changing anything architectural. Never contradict
a logged decision without flagging it to me first.

Keep each doc in the section structure it already has. Update sections in
place rather than appending notes at the bottom.

## After every task
- Tick the task in docs/roadmap.md
- Add a dated entry to docs/progress.md using the entry format at the top of that file
- If you made an architectural or library choice, log it in docs/decisions.md
- Update docs/architecture.md if structure, data model or services changed

## Where knowledge goes
- A fact about the app (stack, data model, a service we use) goes in docs/architecture.md
- A one-line rule that always applies goes in the Rules section below
- A multi-step procedure we repeat (add a route, write a migration, deploy)
  becomes a project skill in .claude/skills/, only with my approval
- Never edit the global framework skills in ~/.claude/skills/

## How to work
- Plan before coding on anything bigger than a small fix
- Write or update tests alongside code; run them before saying a task is done
- Small commits with clear messages, one task per commit
- Ask me when something is ambiguous rather than guessing on product decisions

## Rules
(project-specific one-liners get added here over time)

## Commands
(fill in once the stack exists: dev, test, lint, build, deploy)

## Framework settings
- Skill suggestions: default
