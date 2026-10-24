# claude-framework

A small Claude Code setup for building personal apps. You paste in a messy idea,
it turns that into a plan, and then you build it one task at a time. Claude
remembers where you left off because the project keeps its own notes.

It installs into `~/.claude`, so once it's there it works in every project you open.
If you want the reasoning behind it, read [OVERVIEW.md](OVERVIEW.md).

## What you get

You get eight slash commands:

- `/plan-idea <your brainstorm>` asks you a few questions, then writes a vision,
  an architecture and a roadmap for the app.
- `/build` picks up the next task on the roadmap, writes it with tests, updates the
  notes and commits. Add a task name if you want a specific one. With auto-continue
  on, it then keeps going through the milestone on its own (more on that below).
- `/add-feature <your idea>` reads what's already built, writes a spec for the new
  feature and adds its tasks to the roadmap.
- `/ship` runs a review, the tests and a build, then tags the release and deploys.
- `/project-init` sets up the notes folder from the templates. You rarely need
  to type this one; `/plan-idea` runs it for you.
- `/make-skill <a procedure>` saves something you keep doing in this project,
  like "how we add API routes", as a skill Claude follows from then on.
- `/skill-suggestions on|off|default|status [global]` controls whether Claude
  suggests those skills on its own (more on that below).
- `/auto-continue on|off|default|status [global]` controls whether `/build` moves
  on to the next task by itself.

Plus two subagents (`code-reviewer` and `test-runner`) and a hook in
`settings.json` that shows Claude your latest progress every time a session starts.

The notes Claude keeps in each project (`vision.md`, `architecture.md`,
`roadmap.md` and so on) come from fixed templates in
`skills/project-init/templates/`, so every project's docs have the same sections
and you always know where to look. Claude writes and updates them. You can edit
them whenever you like. If you want different sections, change the templates.

## Project skills

As a project grows you'll do some things over and over: add another API route,
another screen, another migration. After each `/build`, Claude checks whether it
just repeated something it has done before, or whether you've corrected it on
the same thing twice. If so, it asks:

> I've now added 3 API routes the same way. Want me to save that as an
> `add-api-route` skill so the next ones follow the same pattern?

Say yes and it writes the skill into your project's `.claude/skills/` folder
(Claude Code will ask you to approve writing there), lists it in
`architecture.md` and commits it. Say no and it notes that, so it won't ask
again. Facts about your app still go in `architecture.md`, and one-line rules go
in `CLAUDE.md`. Only multi-step procedures become skills. `/ship` checks your
project skills against the code and offers to fix or delete stale ones.

If the suggestions get in the way, turn them off:

- `/skill-suggestions off` turns them off for this project.
- `/skill-suggestions off global` turns them off everywhere by default.
- `/skill-suggestions default` makes this project follow the global setting again.
- `/skill-suggestions status` shows what's in effect.

A project setting always beats the global one. The global default lives in
`~/.claude/framework-settings.md`. `/make-skill` keeps working either way.

## Auto-continue

With auto-continue on (the global default), `/build` doesn't stop after one task.
It only stops to ask you when it really needs you: a product question the docs
don't answer, going against a logged decision, something only you can provide
(keys, sample files), a check it can't get passing, or a skill suggestion. It no
longer waits for you to approve plans; it writes the plan's main choices into
`progress.md` instead, and logs any architectural choice in `decisions.md`.

Claude can't type `/clear` for you, so each next task runs in a fresh subagent,
which starts with empty context just like `/clear`. You see a two or three line
summary per task and can interrupt at any time. The chain stops when the current
milestone is done, so you can look things over before the next one.

- `/auto-continue off` goes back to one task per `/build` for this project, with
  plan approval for bigger tasks.
- `/auto-continue off global` makes that the default everywhere.
- `/auto-continue default` and `/auto-continue status` work like the skill
  suggestion ones.

## Using it

1. Make an empty folder, open it in VS Code and start Claude Code.
2. Type `/plan-idea` and paste your idea. Answer the questions, or just say "defaults".
3. Type `/build`. With auto-continue on it works through the first milestone,
   stopping only when it needs you. With it off, look over what it did, then type
   `/clear` and run `/build` again until the first milestone is done.
4. When you think of something new, use `/add-feature`, then go back to `/build`.
5. When a big chunk of work is finished, run `/ship`.

Claude won't run these commands on its own. You type them when you want them;
the one exception is auto-continue chaining `/build` tasks within a milestone.
You can still chat normally in between, and the project rules make sure any
changes you ask for get written into the notes.

To use a subagent directly, just ask, for example "use the code-reviewer subagent on the last commit".

## Picking a model

`settings.json` is set to `fable`. The first time you use it on a machine, run
`/model fable` and accept the usage-credits prompt.

To switch, run `/model opus` (or edit `"model"` in `settings.json`). The subagents
follow whatever model your main session uses, so that's the only place to change it.
If you resume an old session it keeps its old model, so start a new one after switching.

## Installing it on a computer

These steps are for a Mac. Swap in your own GitHub username wherever you see `USERNAME`.

1. Install VS Code and the Claude Code extension from Anthropic, then sign in to
   your Claude account in the extension.
2. Make a GitHub token. GitHub won't take your password for git, so go to
   https://github.com/settings/tokens/new while signed in to the account that owns
   this repo. Give it a name, tick `repo`, pick an expiry and click Generate.
   Copy it somewhere for a minute, because GitHub only shows it once.
3. If you already have a `~/.claude` folder, move it aside:
   ```
   mv ~/.claude ~/.claude-old 2>/dev/null
   ```
4. Clone the repo into its place:
   ```
   git clone https://USERNAME@github.com/USERNAME/claude-framework.git ~/.claude
   ```
   When it asks for a password, paste the token. Nothing appears while you paste,
   that's normal. Your Mac saves it, so you won't be asked again.
5. If your old folder had permissions you want to keep, copy them from
   `~/.claude-old/settings.json` into the new `settings.json`. Everything else
   (history, caches) Claude Code rebuilds on its own.
6. Restart VS Code, open a project, start Claude Code and run `/model fable`
   (or `/model opus`) once.
7. Type `/`. If you see `plan-idea`, `build`, `add-feature`, `ship`,
   `project-init`, `make-skill`, `skill-suggestions` and `auto-continue`, you're set.

## Keeping more than one computer in sync

When you change a skill, an agent or a setting, push it:
```
cd ~/.claude
git add -A && git commit -m "what you changed" && git push
```
Then pull before you start work on the other computer:
```
cd ~/.claude && git pull
```
The `.gitignore` only lets the framework files in. Your session history,
credentials and the skills synced from your Claude account (`skills/synced/`)
stay off GitHub.

## If something goes wrong

**Git keeps asking for a password, or says permission denied.** Your remote
probably doesn't include your username. Run `git remote -v` to check, and fix it with
`git remote set-url origin https://USERNAME@github.com/USERNAME/claude-framework.git`.
Having the username in there also stops git from grabbing a different GitHub
login saved on the same computer, like a work account.

**Your token expired.** Make a new one and paste it the next time git asks. If
git doesn't ask, open Keychain Access, search for `github.com`, delete the entry
for your username and try again.

**The slash commands aren't showing up.** Restart VS Code. Skills only load when
a session starts.

**You can't select Fable.** Run `/model fable` and accept the prompt. If that
doesn't work, update the Claude Code extension.
