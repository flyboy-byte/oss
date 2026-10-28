# tangency-labs/solidcraft

Pure-Rust parametric CAD app. Agent-driven: contributions by AI agents are expected. Remotes:
`origin` = upstream (read-only; the repo's docs still say `bherbruck/solidcraft`, which moved to
tangency-labs in 2026-10), `fork` = your fork. Read `AGENTS.md` each time: it is long and changes
often. If the checkout has `skills/solidcraft-contributing/SKILL.md`, read it too: it is the
maintainers' own contributing skill and outranks these notes.

## Kernel (2026-10-27)
The kernel on `main` is truck (`crates/kernel` only). Kernel work is paused (#3992) while
tangency-labs/solidcore is evaluated: no new kernel issues, PRs or discussions. A kernel bug's
reproduction goes into solidcore's regression suite as a fixture instead (`repos/solidcore.md`,
"SolidCraft kernel bugs as fixtures"). Check which kernel a code path uses before diagnosing.

## Where outside help fits (maintainers, Discord #contributing, 2026-10-27)
One-off bugs (not part of a larger issue) that a triager has already commented on; and kernel
reproductions as solidcore fixtures. Not triage or duplicate-hunting (automated and covered), and
not kernel direction or backends.

## Claim
`gh pr list --state open --search "#N"`; then empty `claim: #N` commit + draft PR titled
`<summary> (#N)`; lowest PR wins; the bot assigns and labels. Push at least daily: a draft idle
72 h may be taken over. Claim races are real: re-check right before starting.

## Non-negotiables (from AGENTS.md)
Clean-room; never read GPL/LGPL CAD code; no commercial-CAD names in PR or issue text, and no
commercial-CAD icons, data or screenshots. Licence is MIT OR Apache-2.0: reciprocal code (OSL-3.0,
e.g. third-party SolidCraft bridges) may be read for ideas, never copied. Every
asset needs an ATTRIBUTION.md row; no panics/unsafe in non-test code; everything is a command;
layering (`cargo xtask layers`); never break wasm. Design-format changes: one new file in
`crates/doc/src/format/pending/`. Never regenerate `docs/features.md` or
`THIRD-PARTY-LICENSES.txt` in a PR. Keep lists sorted (`cargo xtask lists --fix`).

## CI to mirror
`cargo xtask ci` (fmt, clippy -D warnings, tests, assets, layers, wasm), with
`CARGO_TARGET_DIR=<repo>/target`, under `nice`. On a shared machine wait while load is over 24.

## Reproducing with the MCP server
The headless `solidcraft-cli` opens designs only up to its own format version: check
`solidcraft-cli --version` before opening a reporter's file (format 40 needs 0.24.1+), and copy
the file into the MCP's allowed `/tmp/solidcraft-mcp-*/` folder. A running MCP keeps its old
binary until the session restarts. Direction labels depend on context: "positive" from a body face
can point away from the body, so a cut "with no result" may simply miss; check the bounding box
and the removed volume first.

## Evidence
Observable behaviour needs real screenshots or a recording, before and after if possible: run
the app (`cargo run --release -p solidcraft -- --sample --control PORT`, under `Xvfb :99` if
headless) and drive it over the control channel (`docs/control-protocol.md`). Attach to the PR;
don't commit them.

## PR description
Keep the `Changes a default: no` / `Changes a default: yes — <what>, old behaviour kept behind
preference <name>` line filled in (`pr-checks.yml` checks it).

## Merge queue
A queued PR can be removed with "failed_checks" when `main` moved under it (2026-10: a new field on
`Sel::SketchCurve` broke a new test's struct literal). Read the PR's own queue run
(`gh run list --event merge_group`, branch `gh-readonly-queue/main/pr-N-...`), fix, rebase onto
`origin/main`, push, comment; a maintainer re-queues it.

## Sync
Rebase, never merge `main` in. Don't push only to catch up with `main`; when you push anyway or it
conflicts, rebase onto `origin/main` and `--force-with-lease` your own branch. Don't re-run CI on
an unchanged commit.
