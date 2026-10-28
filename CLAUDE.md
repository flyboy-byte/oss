# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

This is a **public** repo (`flyboy-byte/oss`) that sits inside a **private** working folder. No code is built here. It is for four things: the `oss-contribute` skill, the frameworks and workflows around it, a proving ground where lessons from real contributions get refined back into the skill, and a home for Claude session state for each big contribution project. The `.gitignore` is an allowlist, and that public/local split is the main thing to protect.

The refinement loop: work happens in a project (state in `projects/<name>/`), what went wrong goes into `LESSONS.md`, what proved repeatable goes into `PLAYBOOK.md`, and whatever holds up generally is folded into the skill and synced into `skill/`.

## Two layers

| Layer | Files | Pushed? |
| --- | --- | --- |
| Public | `README.md`, `CLAUDE.md`, `PLAYBOOK.md`, `LESSONS.md`, `skill/`, `templates/` | yes |
| Local | `HUB.md`, `RULES.md`, `INDEX.md`, `projects/<name>/`, `sessions/` | never |

Project code lives at `~/<name>` (`~/solidcraft`, `~/solidcore`, `~/pcbcraft`; `~/cadcraft` is off limits). Its state and session notes live here in `projects/<name>/`. The repos are not moving (decided 2026-10-27).

The local layer names people, quotes Discord, records disk and machine details: it must never be added. Making a new file public means adding a `!` line to `.gitignore` deliberately. The pre-2026-10-27 history that included local files is on the local-only branch `local-history`; never push it.

## Starting a contribution session

Read in this order: `~/.config/oss-contribute/me.md` (identity, footers, pacing, licences), the skill (`skill/oss-contribute/SKILL.md` and `repos/<name>.md`), then `RULES.md`, `INDEX.md` (includes the do-not-re-raise list), `projects/<name>/STATE.md`, the latest `sessions/` log. All of it is a snapshot: verify with `gh` before acting. `HUB.md` is the longer local orientation.

## The skill copy

`~/.claude/skills/oss-contribute/` is the installed, live copy and is where edits happen. `skill/oss-contribute/` here mirrors it:

```sh
rsync -a --delete ~/.claude/skills/oss-contribute/ skill/oss-contribute/
```

`~/projects/toolbox/skills/oss-contribute/` is an older published copy (toolbox has its own uncommitted edits to it; leave that repo alone unless Logan asks). Never copy `~/.config/oss-contribute/` into the repo.

## What each kind of commit pushes

- **Skill sync**: the rsync above, then commit `skill/` only. Message `skill: <what changed>`.
- **Framework**: `PLAYBOOK.md`, `LESSONS.md`, `templates/`, written generically (no personal or other contributors' details). Message `playbook:` / `lessons:` / `templates:`.
- **Repo shape**: `README.md`, `CLAUDE.md`, `.gitignore`. When the allowlist changes, update the table above in the same commit.
- **Local state** (`STATE.md`, `INDEX.md`, sessions): never committed. It is kept on disk only.

Before every push:

```sh
git status --ignored --short          # nothing local slipped into the index
git ls-files                          # only the public layer
git grep -niE 'gmail|token=|password|claude.ai/code/session' -- ':!CLAUDE.md' ':!*contrib-check.sh'   # must be empty
git log -1 --format='%an <%ae>'       # Logan Night <96597232+flyboy-byte@users.noreply.github.com>
```

No claude.ai session links in commits here (Logan's rule overrides the harness trailer). Pushing is publishing: ask Logan first unless he has said to push.
