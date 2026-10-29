<h1 align="center">oss</h1>

<p align="center">
  <strong>Contributing to other people's open-source projects, with Claude Code doing the legwork.</strong><br>
  A skill that makes the AI work the careful way, plus the framework around it<br>
  that lets every session pick up where the last one stopped.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Claude%20Code-skill-d97757" alt="Claude Code skill">
  <img src="https://img.shields.io/badge/merged%20upstream-21%20PRs-2ea44f" alt="21 merged upstream PRs">
  <img src="https://img.shields.io/badge/focus-tangency--labs-6f42c1" alt="Current focus: tangency-labs">
  <img src="https://img.shields.io/badge/scope-any%20OSS%20project-informational" alt="Scope: any open-source project">
  <img src="https://img.shields.io/badge/license-MIT-informational" alt="MIT">
</p>

<p align="center">
  <a href="#track-record"><b>Track record</b></a> •
  <a href="#whats-proven"><b>What's proven</b></a> •
  <a href="#how-it-works"><b>How it works</b></a> •
  <a href="#install"><b>Install</b></a> •
  <a href="#contribute"><b>Contribute</b></a> •
  <a href="#whats-in-the-repo"><b>Layout</b></a>
</p>

---

> [!NOTE]
> **Built on a few big projects first, aimed at all of them.** Right now the work happens in
> [tangency-labs](https://github.com/tangency-labs) (SolidCraft, solidcore, pcbcraft) and
> [storytold/cadcraft](https://github.com/storytold/cadcraft): large, fast-moving Rust CAD/EDA
> projects where many people and agents open PRs every day. That's the hardest setting, so it's
> where the rules get tested. Everything general goes into the skill, which is written for
> **any** open-source project, from a one-line docs fix on a solo repo to a kernel fixture on a
> big one.

## The short version

On someone else's project, **maintainer time is the scarce resource**. PRs land when they're
small, clearly right, proven by tests and written up honestly. AI makes careless PRs cheap, and
projects notice.

[`oss-contribute`](skill/oss-contribute/) is a [Claude Code](https://claude.com/claude-code)
skill that holds Claude to that bar. It reads the project's rules first, searches for
duplicate work, reproduces the bug first, proves each test fails without the fix, runs exactly
what CI runs, keeps the PR in draft until the evidence is in, and asks before anything public.
Around it, this repo keeps the **framework**: per-project state, session handoffs, a lessons
inbox, and a playbook where new workflows are proven before they go into the skill.

---

## Track record

All written with Claude Code; the skill grew out of them and was refined on each one. Links go to the live PR lists.

| Project | What it is | Merged | Examples |
|---|---|:---:|---|
| [**solidcore**](https://github.com/tangency-labs/solidcore/pulls?q=is%3Apr+is%3Amerged+author%3Aflyboy-byte) | Rust port of NVIDIA's SMLib B-rep kernel | **11** | uninitialised Jacobian upstream ([#258](https://github.com/tangency-labs/solidcore/pull/258)), fillet corner setback ([#1193](https://github.com/tangency-labs/solidcore/pull/1193)) |
| [**solidcraft**](https://github.com/tangency-labs/solidcraft/pulls?q=is%3Apr+is%3Amerged+author%3Aflyboy-byte) | Parametric 3D CAD app in pure Rust | **9** | chamfer manipulator ([#2856](https://github.com/tangency-labs/solidcraft/pull/2856)), negative drag arrows ([#5508](https://github.com/tangency-labs/solidcraft/pull/5508)) |
| [**cadcraft**](https://github.com/storytold/cadcraft/pulls?q=is%3Apr+is%3Amerged+author%3Aflyboy-byte) | Clean-room 2D CAD/drafting app in Rust | **1** | bare system variable runs SETVAR ([#799](https://github.com/storytold/cadcraft/pull/799)) |
| [**pcbcraft**](https://github.com/tangency-labs/pcbcraft) | MCP-first Rust EDA, KiCad-compatible | **0** | not started: a profile stub, nothing contributed yet |

---

## What's proven

What has actually worked on real PRs, versus what is written down but not yet exercised.

| | Piece | Notes |
|---|---|---|
| ✅ | **Core fix workflow** | Claim → reproduce → smallest fix → tests that fail without it → CI mirror → honest PR. Refined on real merged PRs. |
| ✅ | **Review handling** | Human and bot reviews verified before applying; no pushes after approval (learned from a merge queue). |
| ✅ | **Repo profiles** | `solidcraft`, `solidcore`, `cadcraft`: claim rules, CI commands, non-negotiables. Built from real PRs. |
| ✅ | **Kernel bug → regression fixture** | Turning a CAD-kernel issue into a solidcore test with a closed-form expected answer. Used on real solidcore PRs. |
| ✅ | **Resuming and handoffs** | Read order, a NOW board, first commands and a [handoff template](templates/handoff.md). Used to restart long sessions after context compaction. |
| 🚧 | **Small repos, GitLab, Codeberg, email patches, DCO/CLA** | Written in [`workflows.md`](skill/oss-contribute/reference/workflows.md). Not yet used outside GitHub. |
| 🚧 | **pcbcraft** | Profile stub only. GPL-3.0, unlike its siblings. |

---

## How it works

Two loops. The inner one is a single contribution; the outer one is how the skill gets better.

```
  ONE CONTRIBUTION                              REFINEMENT
  ────────────────                              ──────────
  read the project's rules                      real PR, real review
          │                                            │
  pick a triaged issue, search for dupes               ▼
          │                                     something went wrong ─▶ LESSONS.md
  claim (draft PR, the project's way)           something repeatable ─▶ PLAYBOOK.md
          │                                            │
  reproduce on a fresh main                      holds up on 2+ cases
          │                                            │
  fix ─▶ revert ─▶ test must fail ─▶ restore           ▼
          │                                     folded into skill/,
  run exactly what CI runs                      struck through where it was
          │
  ready for review ─▶ answer reviews ─▶ merged
```

Each project's code stays in its own clone (`~/<repo>`). Its working state (open PRs, findings,
session logs, evidence) lives in one hub folder, this one, so a new session reads a few files
and carries on instead of re-deriving the last one.

> [!IMPORTANT]
> **Asks before anything public.** Forking, pushing, commenting, marking a PR ready: the skill
> treats each one as the user's call, one at a time. Commits use the GitHub noreply address, and
> Claude session links stay out unless you allow them.

---

## Install

```sh
git clone https://github.com/flyboy-byte/oss.git
mkdir -p ~/.claude/skills
cp -r oss/skill/oss-contribute ~/.claude/skills/
```

Then, in Claude Code inside a clone of the project you want to help:

```
find a good first issue in this repo and fix it
open a PR for this
answer the review on my PR
```

On first use Claude asks for your name, noreply email and PR limit, and saves them in
`~/.config/oss-contribute/`, outside the skill. The [skill's README](skill/oss-contribute/README.md)
has the details.

<details>
<summary><b>Using the hub framework for your own projects</b></summary>

<br>

Copy `templates/` into a folder of your own and keep one `projects/<name>/` per project you
contribute to:

| File | From | Holds |
|---|---|---|
| `projects/<name>/STATE.md` | [`templates/project/STATE.md`](templates/project/STATE.md) | Who owns what, open threads, findings with the commit they were measured on |
| `sessions/YYYY-MM-DD.md` | [`templates/session.md`](templates/session.md) | What shipped, the timeline, decisions the owner made |
| `HANDOFF.md` | [`templates/handoff.md`](templates/handoff.md) | Open PRs, next steps, do-not-re-raise, a paste-back block for a fresh session |

Point the skill at it from `~/.config/oss-contribute/me.md`, and its "Resuming" section does
the rest.

</details>

<details>
<summary><b>Adding a new project</b></summary>

<br>

The skill builds a profile on first use with
[`reference/new-repo.md`](skill/oss-contribute/reference/new-repo.md): AI policy, how work is
claimed, the exact CI commands, licence, and the project's non-negotiables. It saves it as
`repos/<name>.md` from [`_template.md`](skill/oss-contribute/repos/_template.md). Where a profile
disagrees with the general rules, the profile wins.

</details>

---

## Contribute

> [!TIP]
> **Anything is welcome**, from a typo to a new workflow. You don't need to be an expert or to
> have merged a PR anywhere. Half-finished is fine; open a draft.

| You have... | Go to |
|---|---|
| A mistake your agent made, and what fixed it | add it to [`LESSONS.md`](LESSONS.md) |
| A workflow that worked more than once | [`PLAYBOOK.md`](PLAYBOOK.md), under "Being tried" |
| A project you contribute to | a profile in [`skill/oss-contribute/repos/`](skill/oss-contribute/repos/) |
| A step in the skill that is wrong or missing | a PR, or an issue if unsure |
| A question or "this didn't work for me" | an [issue](https://github.com/flyboy-byte/oss/issues/new/choose) |

Details, and what makes a PR land, are in [`CONTRIBUTING.md`](CONTRIBUTING.md). Changes to
`SKILL.md` are read line by line, because they change how every user's agent behaves.

---

## What's in the repo

| Path | What it is |
|---|---|
| [`skill/oss-contribute/`](skill/oss-contribute/) | The skill: [`SKILL.md`](skill/oss-contribute/SKILL.md), the general [playbook](skill/oss-contribute/reference/workflows.md), per-project [profiles](skill/oss-contribute/repos/), and [`contrib-check.sh`](skill/oss-contribute/scripts/contrib-check.sh) for the pre-push checks |
| [`PLAYBOOK.md`](PLAYBOOK.md) | Workflows still being proven, before they move into the skill |
| [`LESSONS.md`](LESSONS.md) | The lessons inbox: each mistake, and where in the skill it was fixed |
| [`templates/`](templates/) | State, session and handoff templates for the hub |
| [`CONTRIBUTING.md`](CONTRIBUTING.md), [`.github/`](.github/) | How to contribute, the PR template and issue templates |
| [`CLAUDE.md`](CLAUDE.md) | How Claude should work in this repo, including what may and may not be published |

Live per-project state and session logs stay local: `.gitignore` is an allowlist, so nothing
personal gets published by accident.

[MIT](LICENSE)
