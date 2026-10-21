---
name: add-feature
description: Turn a brainstorm for a new feature into a spec that fits the existing app, and add its tasks to the roadmap.
argument-hint: <paste your feature brainstorm>
disable-model-invocation: true
---

Feature brainstorm:

$ARGUMENTS

1. Read docs/vision.md, docs/architecture.md, docs/decisions.md and
   docs/progress.md so you know what already exists and why.
2. Explore the relevant code. Use the Explore subagent for anything broad
   so the main context stays clean.
3. Ask up to 3 clarifying questions, each with a sensible default.
4. Copy docs/features/_template.md to docs/features/<feature-name>.md (if the
   template is missing, use ~/.claude/skills/project-init/templates/feature.md)
   and fill in every section: goal, user flow, data changes, files it touches,
   risks or conflicts with past decisions, and out of scope.
5. Add its tasks to docs/roadmap.md under a new milestone or in the right
   place, each sized for one session, in build order.
6. Log any new decisions in docs/decisions.md and add a progress.md entry.
7. Commit with the message "Plan feature: <feature-name>".

Do not write application code yet. End by saying `/build` will start it.
