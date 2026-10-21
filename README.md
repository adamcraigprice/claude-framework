# claude-framework

A small Claude Code setup for building personal apps. You paste in a messy idea,
it turns that into a plan, and then you build it one task at a time. Claude
remembers where you left off because the project keeps its own notes.

It installs into `~/.claude`, so once it's there it works in every project you open.
If you want the reasoning behind it, read [OVERVIEW.md](OVERVIEW.md).

## What you get

You get five slash commands:

- `/plan-idea <your brainstorm>` asks you a few questions, then writes a vision,
  an architecture and a roadmap for the app.
- `/build` picks up the next task on the roadmap, writes it with tests, updates the
  notes and commits. Add a task name if you want a specific one.
- `/add-feature <your idea>` reads what's already built, writes a spec for the new
  feature and adds its tasks to the roadmap.
- `/ship` runs a review, the tests and a build, then tags the release and deploys.
- `/project-init` sets up the notes folder. You rarely need to type this one;
  `/plan-idea` runs it for you.

Plus two subagents (`code-reviewer` and `test-runner`) and a hook in
`settings.json` that shows Claude your latest progress every time a session starts.

## Using it

1. Make an empty folder, open it in VS Code and start Claude Code.
2. Type `/plan-idea` and paste your idea. Answer the questions, or just say "defaults".
3. Type `/build`. Look over what it did, then type `/clear` and run `/build` again.
   Keep going until the first milestone is done.
4. When you think of something new, use `/add-feature`, then go back to `/build`.
5. When a big chunk of work is finished, run `/ship`.

Claude won't run these commands on its own. You type them when you want them.
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
7. Type `/`. If you see `plan-idea`, `build`, `add-feature`, `ship` and
   `project-init`, you're set.

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
