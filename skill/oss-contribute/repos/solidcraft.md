# tangency-labs/solidcraft

Pure-Rust parametric CAD app. Agent-driven: contributions by AI agents are expected. Remotes:
`origin` = upstream (read-only; the repo's docs may still name its old owner, before it moved to
tangency-labs in 2026-10), `fork` = your fork. Read `AGENTS.md` each time: it is long and changes
often. If the checkout has `skills/solidcraft-contributing/SKILL.md`, read it too: it is the
project's own contributing skill, written for first-time contributors. It is a second source, not a
boss: where it and these notes differ, check `AGENTS.md`, `CONTRIBUTING.md` and recent merged PRs
for what is current, and tell the user. Refined copies of parts of it are at the bottom of this file.

## Kernel (2026-10-27)
The kernel on `main` is truck (`crates/kernel` only). Kernel work is paused (#3992) while
tangency-labs/solidcore is evaluated: no new kernel issues, PRs or discussions. A kernel bug's
reproduction goes into solidcore's regression suite as a fixture instead (`repos/solidcore.md`,
"SolidCraft kernel bugs as fixtures"). Check which kernel a code path uses before diagnosing.

## Where outside help fits (maintainers, the project's contributing channel, 2026-10-27)
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

## Draft PRs, bot labels and new UI text (2026-10-28)
- CI skips draft PRs and selects its jobs by changed paths. Mark a finished claim ready at once.
- The kernel-paused bot matches title words (`scripts/kernel-paused/kernel-paused.js`, `TOPIC`:
  fillet, chamfer, offset, boolean, step, loft and more) as well as kernel paths. A sketch or UI PR
  with one in its title waits for a maintainer, so keep those words out of titles unless it is
  kernel work, and don't ping about the label.
- New UI text must carry ar, he, ja, vi and zh (`COMPLETE` in `i18n.rs`); run
  `cargo xtask i18n --check` before pushing.
- A new toolbar command ships with its help tooltip; the `BACKLOG` in
  `crates/ui-egui/src/command_help.rs` only shrinks.
- A red `ci` check can be a cancelled earlier run; read the newest run for the head commit.

## When the bug turns out to be in the kernel
Reproduce on current `main` with an engine test, find the cause, and stop: no PR (#3992). Post the
reproduction, the cause and where the fix belongs on the issue (with the user's OK), and keep the
test as a patch outside the repo (example: #4732, a revolved profile with an edge on the axis
leaves its flat faces as non-planar sectors, so Create Sketch refuses them; the fix belongs in
`revolve_touching_axis`, `crates/kernel/src/build.rs`).

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

## Driving the app
Run a build with its own settings (`XDG_DATA_HOME` and `XDG_CONFIG_HOME` set to a scratch folder)
and `--control PORT`; the MCP tools connect through the token the app writes under its data folder
(`solidcraft/control/PORT.token`), which has to be in the default folder for them to find it.
Per-worktree `target/` folders and their `debug/incremental` are the big disk cost: share one
`CARGO_TARGET_DIR` and clear `debug/incremental` between jobs.

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

---

# From the project's own skills (refined copies, kept separate)

Borrowed 2026-10-29 from `skills/` on `origin/main` d4fb5538 (`solidcraft-contributing`,
`-reporting`, `-tests`, `-ground-rules`, `-crash-safety`), reworded and cut down. SolidCraft is
Apache-2.0 (MIT OR Apache-2.0); the originals are the source, and they change, so read the
current ones in a checkout. Nothing above this line was changed to match them. Marks: **new** =
nothing like it in our notes; **same** = we already say it, listed so you can compare.

## Contributing
- **new:** before claiming, list open PRs with the files they change
  (`gh pr list -R O/R --state open --limit 500 --json number,title,files`), not only a search for
  the issue number. The same feature turns up under another number.
- **new:** the gate reads the working tree. Don't switch branches while `cargo xtask ci` runs: it
  compiles the other branch's code and fails on a change that isn't in your PR.
- **new:** if your claim lost to a lower-numbered PR, close yours (ask the user first).
- **new:** capture evidence yourself; don't ask a person for a screenshot you can make.
  `solidcraft-cli snapshot script.json --out shot.png [--view iso]` renders a command script with
  no display (it fails if a step fails, so snapshot the script without the failing step).
- **new:** update `docs/roadmap-detail.md` when a milestone task lands.
- **same:** claim by draft PR, daily pushes, `Changes a default:` line, don't push to catch up with
  `main`, rebase and `--force-with-lease`, never merge `main` in, load check before a big build.

## Reporting a bug (their `solidcraft-reporting`)
- **new:** reproduce in a fresh design, not the user's file. Headless: write the steps as
  `{"commands":[...]}` and `solidcraft-cli run repro.json` (a failing feature fails the run and
  names it). Over MCP: `new_design`, then one `batch` with `rollback: false` so the steps before
  the failure stay for inspection.
- **new:** minimise while the same message still appears: drop steps from the end, then the middle;
  swap sketches for `solid.box`/`solid.cylinder`; round the numbers; stop when removing more makes
  it pass. Record the boundary ("fails at radius 5, works at 4.9").
- **new:** cap runs that may hang (`timeout 120 solidcraft-cli run repro.json --timings`; a killed
  run prints nothing, so run one step longer at a time). `batch` with `timings: true` times each step.
- **new:** check the claim with a number: `measure` against a value worked out by hand, and put
  both in the report. `body_topology` shows what was there to pick.
- **new:** privacy: never attach or quote the user's design or a crash report's `design.sc` unless
  they agreed; use a small public-safe example.
- **new:** the title and body report what was observed ("the fillet fails with `...`"), never a
  guessed cause. (Ours says this too; theirs makes it a template with Steps, Minimal script,
  Expected, Actual, how often, Attachments.)
- **new:** the report states the version it was written against; ask for it (Help, About, or
  `solidcraft-cli --version`).

## Tests (their `solidcraft-tests`)
- **new:** a regression test goes in the area's existing test file, never a new file named after
  the bug.
- **new:** a UI scenario that records a bug before its fix carries `{"pending": "why"}`; remove it
  in the fixing PR. A Rust test that fails until a fix lands is `#[ignore = "fails until #N is
  fixed"]`. A failing test with no fix can't merge: it turns `main` red.
- **new:** reference (oracle) tests: `tests/oracle/<case>/recipe.json` + `expected.json`, run by
  `cargo xtask oracle`; expected values come from a closed form (`tools/oracle-analytic/`), never
  by changing the expected value to match SolidCraft's output.
- **new:** `cargo xtask test` uses the `ci` profile (debug assertions off on purpose: truck's
  reject valid geometry). If a test fails only in a plain debug build, say so in the PR.
  `SOLIDCRAFT_TEST_JOBS=N` bounds load on a shared machine.
- **new:** PRs skip a few heavy tests that run in the merge queue and on `main`
  (`.config/nextest.toml`); check them after the merge. Goldens: re-record in the same PR
  (`ROBUSTNESS_GOLDENS_BLESS="why"`); the action matrix is a ratchet (`--bless`).

## Never crash (their `solidcraft-crash-safety`)
- **new, matters to us:** crashes and data loss are taken as issues **even while kernel work is
  paused** (#3992, CONTRIBUTING "Kernel work is paused"). A reproducible crash on current `main`
  isn't covered by our kernel-pause stop rule; check the issue first.
- **new:** the app records every panic to a local crash folder. `solidcraft-cli crashes`,
  `crashes show <id>`, `crashes replay <id>`, or the MCP `crash_reports` tool give version, command
  and replay; a crash fix starts from that replay as its regression test.
- **same:** no `unwrap`/`expect`/`panic`/`unsafe` in non-test code; errors are `Result`.

## Ground rules (their `solidcraft-ground-rules`)
- **same:** clean-room, no GPL/LGPL/AGPL reading, no commercial-CAD data, assets need an
  ATTRIBUTION.md row.
- **new:** the GPL oracle is run as a separate program through its public scripting API to compute
  reference values (`tools/freecad-oracle/`); nothing is linked or shipped. Running a tool as a
  black box differs from reading its source.
- **new:** terms come from the glossary (`docs/book/src/reference/glossary.md`); a new idea adds its term.
- **new:** the storytold projects are named as references for implementation conventions (relevant
  when we look at cadcraft).

## Not borrowed
Their first-time-contributor script (check the environment, fork, show every public action to the
person) is written for newcomers; it is the same job as our "ask before public writes" and the user's
standing OKs, so it stays theirs.
