# Adam's Claude Code framework

Personal, project-agnostic setup for building apps from scratch with Claude Code.
It lives in `~/.claude`, so it works in every project automatically.
For the why behind it, read [OVERVIEW.md](OVERVIEW.md).

## What's in here
| Path | What it is |
|---|---|
| `skills/plan-idea` | `/plan-idea <brainstorm>` turns a raw idea into vision, architecture and roadmap |
| `skills/build` | `/build [task]` builds the next roadmap task with tests, then updates the docs |
| `skills/add-feature` | `/add-feature <brainstorm>` specs a new feature against what already exists |
| `skills/ship` | `/ship` reviews, tests, tags and deploys after a major update |
| `skills/project-init` | `/project-init` scaffolds CLAUDE.md + docs/ (runs automatically when needed) |
| `agents/code-reviewer.md` | Fresh-eyes, read-only reviewer subagent |
| `agents/test-runner.md` | Runs tests/build and diagnoses failures |
| `settings.json` | Model choice + SessionStart hook that loads project memory |

## Daily use
1. Open an empty folder in VS Code and start Claude Code.
2. `/plan-idea` then paste your brainstorm. Answer its questions (or say "defaults").
3. `/build` to do the next roadmap task. Check it, then `/clear`. Repeat.
4. New idea later? `/add-feature` then paste it. Then keep using `/build`.
5. After a big update, `/ship`.

The four main commands are manual on purpose: Claude never runs them on its own,
you type them. Subagents get used by `/ship`, or ask for them directly
("use the code-reviewer subagent on this").

## Switching models
- Right now: Fable. The first time on any machine, run `/model fable` once to accept
  the usage-credits prompt.
- When credits run out: run `/model opus` (or change `"model"` in `settings.json`).
  Subagents use `model: inherit`, so they follow automatically.
- Resumed old sessions keep their old model, so start a fresh session after switching.

## Setting up a new computer (step by step)
This works on any Mac. GitHub needs a token instead of a password for git.

1. **Install Claude Code.** Install VS Code and the "Claude Code" extension by
   Anthropic, and sign in to your Claude account inside it.
2. **Make a GitHub token.** In a browser signed in as `adamcraigprice`, go to
   https://github.com/settings/tokens/new , name it after the computer, tick `repo`,
   pick an expiry, click Generate token, and copy it. You only see it once.
3. **Move any existing Claude config out of the way.** In Terminal:
   ```
   mv ~/.claude ~/.claude-old 2>/dev/null
   ```
4. **Clone the framework into place:**
   ```
   git clone https://adamcraigprice@github.com/adamcraigprice/claude-framework.git ~/.claude
   ```
   When it asks for a password, paste the token (nothing shows while you paste).
   macOS saves it in the keychain, so you won't be asked again on this machine.
5. **Bring back anything personal from the old folder** (optional). Claude Code
   recreates its caches and history by itself, so usually nothing is needed.
   If you had extra permissions you liked, copy them from
   `~/.claude-old/settings.json` into `~/.claude/settings.json`.
6. **Restart VS Code**, open a project folder, start Claude Code, and run
   `/model fable` (or `/model opus`) once.
7. **Check it worked:** type `/` and you should see `plan-idea`, `build`,
   `add-feature`, `ship` and `project-init` in the list.

## Keeping computers in sync
After changing a skill, agent or setting on either machine:
```
cd ~/.claude
git add -A && git commit -m "describe the change" && git push
```
Before working on the other machine:
```
cd ~/.claude && git pull
```
Only the framework files are tracked (see `.gitignore`). Sessions, history,
credentials and account-synced skills (`skills/synced/`) never go into git.

## Troubleshooting
- **Push asks for a password every time, or says permission denied:** the remote
  must include your username (`https://adamcraigprice@github.com/...`). Check with
  `git remote -v`. Fix with
  `git remote set-url origin https://adamcraigprice@github.com/adamcraigprice/claude-framework.git`.
  This also stops git from using a work GitHub login saved on the same machine.
- **Token expired:** make a new one (step 2), then run any `git pull`/`git push`
  and paste it when asked. If it doesn't ask, open Keychain Access, search
  `github.com`, delete the `adamcraigprice` entry, and try again.
- **Slash commands don't show up:** restart VS Code. Skills load when a session starts.
- **Fable can't be selected:** run `/model fable` and accept the prompt, or update
  the Claude Code extension.
