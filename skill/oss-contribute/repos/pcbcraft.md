# wdunn001/pcbcraft

MCP-first Rust EDA app (single-window tabbed editor, KiCad compatibility, craft-libs). GPL-3.0,
unlike its MIT/Apache siblings. **The project's home is the `wdunn001/pcbcraft` fork** (its "Start
here" issue, #1, says so). The old tracker `tangency-labs/pcbcraft` is frozen: do not post, comment
or open PRs there. Nobody from this setup has contributed yet; the sections below come from issue #1
only. Read the repo's own README, AGENTS.md, CONTRIBUTING.md and `docs/architecture.md` once, then
correct anything here that they contradict.

## Overrides

Tracker is `wdunn001/pcbcraft` (not tangency-labs). No attribution lines in commits or PR bodies. Own
claim format. Merge `main` in, never rebase. Details below, from issue #1 (written by the owner).

## AI policy

Built for agents: issue #1 is written for them (claims name an `<agent>@<team>`, "if you run several
agents at once"). One rule runs the other way: **no AI attribution lines in commits or PR bodies.**
That overrides this skill's default `Co-Authored-By` trailer and "Generated with Claude Code" footer
here (the skill already says "unless the project says otherwise"). Leave both out.

## Claim

Work only on issues labelled `ready` and `status:open`; every issue body starts with a `**Tags:**`
line that matches the labels. Prefer `conflict:parallel-safe`; `conflict:ui-egui-lib` issues touch
shared UI files, so take one at a time. Read the body and every `### Research` and `### Enrichment`
comment first, and check no live claim exists. Claim with one comment and set `status:claimed` on the
Tags line:

```
CLAIM <agent>@<team> until <UTC time> branch <name>
```

Leases: 6 hours for code, 2 hours for research, enrichment and review. Renew with
`CLAIM-RENEW <agent>`; an expired claim may be taken over. Issues tagged `stage:research` or
`stage:enrich` need that work before coding, and a `### Research` / `### Enrichment` comment (starting
`Base: fork/main at <sha>`) is a welcome contribution on its own. Moved issues say
`Migrated from tangency-labs/pcbcraft#N` in the body; search that text to find an old number.

```sh
gh issue list -R wdunn001/pcbcraft --state open --label ready --label status:open
gh issue list -R wdunn001/pcbcraft --state open --label priority:owner-top
```

## Non-negotiables

- **Engine first:** every design change is a typed engine command with bounded validation and undo.
  No `unwrap`, `expect` or `panic` in production code. Lower crates never depend on the UI.
- **Tests:** an `a_newcomer_*` end-to-end story for anything a user does, plus unit or property
  tests. Every visible control needs a tooltip and a story (CI fails otherwise). Generated outputs
  (Gerber, drill, placement, BOM) get golden-file tests. No network in tests: recorded fixtures.
- **UI changes:** before and after screenshots in the PR body. Strings go in
  `crates/text/strings/en.txt`, never inline. No icons, dots or arrows as text characters (fonts may
  lack them): use the icon set or painted shapes.
- **Text:** plain, short sentences. **Never name other EDA tools** in code, docs, commits, issues or
  comments. That includes KiCad in anything posted to that repo, even though this profile may.
- **Licences:** adapted GPL-compatible code goes in ATTRIBUTION.md. No third-party designs,
  datasheets or models unless their licence allows it and the attribution file says so. Check the
  repo's rules before reading KiCad source; running `kicad-cli` as a black-box reference is the
  safe default, but do not mention it in what you post.
- **Known flaky tests** (a single failure is not yours): the language byte-identity check, the
  library folder test, the two frame-budget tests.

## CI to mirror

GitHub Actions on this fork. A change is **done when it is merged into the fork's `main` and the
Actions build on `main` is green**, not when the PR is green. Read `.github/workflows/` for the
exact commands and run them before pushing; this file does not have them yet.

## Sync

Branch from the fork's `main`: `git checkout -b feat-<slug> origin/main`. One clone per agent, never
a shared checkout. Before your last push, **merge** the latest `main` into the branch (not rebase)
and resolve conflicts on your side. If your work depends on an open PR, branch from that PR's branch
and say so in your PR. Never push to `main`.

## Review

PR against `main` on `wdunn001/pcbcraft`. The body is the build report: what was built, files
touched, tests added, limits, and an `Issues: #N` line. Wait for the Actions run to go green. An
integrator verifies and merges with a merge commit; **never merge your own PR.** Questions go in a
comment on the relevant issue.

## Hotspots and gotchas

- UI direction is an owner decision (issue #1 section 5): tabs are Files, Library, Schematic, Layout,
  Manufacture, with 3D as a 2D | 3D toggle inside Layout and no left nav rail. Each tab owns its
  toolbar, asides and bottom sheet (`crates/ui-egui/src/workspace.rs`). Menus use the shared
  component in `ui_kit/menu.rs`. File formats come from one registry in `crates/io`.
- Useful app flags for checking work: `--sample`, `--project examples/showcase.pcbcraft`,
  `--tab <files|schematic|pcb|manufacture>`, `--select <id>`, `--size 1600x1000`,
  `--capture <file.png>` (the screenshot for a PR body).
- The roadmap is `ROADMAP.md`; check open PRs (`gh pr list -R wdunn001/pcbcraft`) before starting.
