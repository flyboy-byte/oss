# oss

How I contribute to other people's open-source projects with [Claude Code](https://claude.com/claude-code) doing the work: the `oss-contribute` skill, and the small framework around it that lets a fresh session pick up where the last one stopped.

## What's here

| Path | What it is |
| --- | --- |
| [`skill/oss-contribute/`](skill/oss-contribute/) | The skill: claim, reproduce, fix, prove with tests, open an honest PR, answer reviews. See its [README](skill/oss-contribute/README.md). |
| [`PLAYBOOK.md`](PLAYBOOK.md) | A worked, repeatable workflow: turning a CAD-kernel bug report into a regression fixture with a closed-form expected answer. |
| [`LESSONS.md`](LESSONS.md) | Mistakes made along the way, so the next session doesn't repeat them. |
| [`templates/`](templates/) | Empty versions of the per-project state file and the session log. |

## The framework

Each project's code stays in its own clone (`~/<repo>`). A single hub folder (this one) holds the working state across all of them:

```
~/projects/oss/
  README.md CLAUDE.md PLAYBOOK.md LESSONS.md   public
  skill/ templates/                            public
  HUB.md RULES.md INDEX.md                     local: orientation, standing rules, do-not-re-raise
  projects/<name>/STATE.md                     local: live state for ~/<name>
  projects/<name>/sessions/                    local: per-project notes
  sessions/YYYY-MM-DD.md                       local: one log per working session
```

The local files never get published. `.gitignore` is an allowlist, so a new file stays private until it is added on purpose. Personal skill settings (name, noreply email, PR limits) live in `~/.config/oss-contribute/`, outside both the skill and this repo.

## Install the skill

```sh
git clone https://github.com/flyboy-byte/oss.git
mkdir -p ~/.claude/skills
cp -r oss/skill/oss-contribute ~/.claude/skills/
```

Then, inside a clone of the project you want to help: "find a good first issue in this repo and fix it". The skill's README covers first-run setup.
