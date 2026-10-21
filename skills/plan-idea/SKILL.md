---
name: plan-idea
description: Turn a raw brainstorm into a vision, architecture and roadmap for a new app. Use when starting a project from scratch.
argument-hint: <paste your brainstorm>
disable-model-invocation: true
---

The user is pasting a raw brainstorm for a new app:

$ARGUMENTS

0. If CLAUDE.md or docs/ is missing in this repo, follow the project-init
   skill first (create only what's missing), then continue.
1. Summarise the core idea back in 3-4 sentences so I can confirm you got it.
2. Ask at most 5 clarifying questions, only about things that change the
   build (platform, auth, data storage, must-have vs nice-to-have). Give a
   sensible default for each so I can just reply "defaults".
3. Propose a stack suited to a solo personal project: simple to host, few
   moving parts, good free tiers. Explain the choice in 2-3 lines and name
   one alternative you considered.
4. After I confirm, fill in the docs. Each doc already has its sections and
   entry formats from the template; fill those sections in place and follow
   the formats exactly:
   - vision.md: problem, users, core features, explicit out-of-scope list
   - architecture.md: stack, folder layout, data model, key patterns
   - roadmap.md: milestones (Milestone 1 = smallest usable MVP), each split
     into checkbox tasks small enough to finish in one session, in build order
   - decisions.md: dated entries for the stack choice and every assumption
   - progress.md: dated entry "Project planned"
5. Fill in the Commands section of CLAUDE.md for the chosen stack.
6. Commit with the message "Plan project".

Do not write application code in this step. End by telling me the first
roadmap task and that `/build` will start it.
