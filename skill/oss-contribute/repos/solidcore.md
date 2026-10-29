# tangency-labs/solidcore

Rust port of NVIDIA SMLib (the geometry kernel SolidCraft will use). Agent-driven: contributions
by AI agents are expected. Remotes: `origin` = tangency-labs (read-only), `fork` = your fork.

## Claim
Empty `claim: #N` commit + draft PR from your fork, title ends `(#N)`; lowest PR number wins.
Most porting units already have claims; bugs and findings are a better way in.

## Correct means "as upstream" by default
Behaviour must match the pinned C++ reference by default, bugs included (usd-brep v1.0.1 since
#1277, 2026-10-27; `c9c5979` before). Upstream bugs are
recorded one file each under `plan/upstream-issues/<ID>--<slug>.md` (`UB-n` undefined behaviour,
`HANG-n`, `WR-n` wrong result, `DOC-n`), findings under `plan/findings/`; never append to a shared
file, and update a merged record with an addendum file. Take the next free number after a real
`git fetch origin`; if two PRs collide, whoever merges second renumbers. `plan/PROGRESS.md` is
generated.

Fixes (policy from #1018 and #1077): crash, undefined-behaviour and hang fixes are unconditional;
wrong-result fixes go behind an opt-in field of `Kernel::fixes` (`UpstreamFixes`, default off),
named after its record (`WR-n`, #issue), read as `k.fixes.<name>`. The difftest compares the
default (upstream) behaviour, so a default-off fix must leave every compared result unchanged.

## CI to mirror
- `cargo fmt --all --check`, plus `rustfmt --edition 2021 --check` by hand on `include!`d files
  (e.g. `crates/difftest/src/units/curve_curve_p9.rs`), which `cargo fmt` skips.
- `cargo clippy --workspace --all-targets -- -D warnings` **and** again with
  `--features difftest/cpp`.
- `cargo test --workspace`; wasm32 check.
- difftest: `cargo run --profile difftest -p difftest --features cpp -- --green` (25-40 min on a
  laptop) must stay at 0 failed; for one unit `--unit <name> --seeds 30000`.
- The C++ reference needs `upstream/` (usd-brep) and `harness/cpp/build.sh --jobs 4`.
- CI pins a Rust version (`rust-toolchain.toml`, 1.98 as of 2026-10); newer local toolchains add
  lints in untouched files (1.99: `clippy::assert_is_empty`, `clippy::explicit_iter_loop`,
  `clippy::mut_mut`, `deprecated`) that stop clippy before it reaches `difftest`. Without rustup, allow those lints (`-A clippy::...`) so the run covers every crate.
- No sccache: `RUSTC_WRAPPER` unset; cap `CARGO_BUILD_JOBS` on a small machine.
- `reports/difftest/*.json` are tracked and rewritten by runs: `git checkout -- reports/`.

## Sync
Merge `origin/main` into the branch rather than rebasing a branch others may have pulled. After a
reference bump (#1277) open PRs must merge `main`; `scripts/fetch-cpp.sh` fetches the new prebuilt. A
single-commit PR that only you work on can be rebased onto `main` (done for #1160 after #1157
merged, no objection).

## Fork fixes
Many issues are written by the maintainers with the fork's C++ diff and a capi test attached
(tangency-labs/smlib). Port the change, gated as above, and build a Rust test that reproduces the
issue's reported failure (the capi helpers, e.g. screw sweeps, often have no Rust builder; build
a small surface or B-rep with the same shape of problem). Issues often say "the Rust port needs
the same change". The parity target is still the pinned upstream reference: trace upstream on the inputs the
fork's fix refuses, port only what upstream gets wrong (UB), and check whether the port's checked
indexing already refuses it. Copying a fork guard verbatim broke 14 `primitive.api_primitives` cases
once (2026-10). Docs-only mismatches go in the `DOC-` series.

## Review
The `tangency-monitor` bot reviews as PR comments ("Reviewed and good to merge once CI is
green"). Maintainers land PRs in batch PRs that merge each PR's reviewed head sha, so the head
rule is strict: a push after review (even a renumber the maintainer offered to do) leaves the PR
out of the batch. Fork PR CI needs maintainer approval (`action_required`). The maintainers' automated first-pass review
approves a specific commit: a later push needs a comment saying what changed. Other contributors'
agents review too and offer scope splits; answer them in the thread. Since 2026-10-21 PRs land through GitHub's merge
queue: once queued, push nothing; a queue run builds main + every PR ahead of yours, so when it fails,
look at the other queue runs before assuming it's your PR.

## Status docs
`plan/STATUS.md` is a snapshot and can be weeks behind `main`; audit units from
`plan/units/*.toml`. Ask the maintainers before calling a mismatch a defect.

## Cleanup
The cargo target dir is `bin/`, not `target/`, and reaches 30-40 GB with the debug, difftest and
C++ builds. Delete build output and worktrees with `cargo xtask rm <path>` and
`cargo xtask worktree-rm <dir>` (AGENTS.md "Deleting", #1313), not `rm -rf`, with the user's OK.

## SolidCraft kernel bugs as fixtures
SolidCraft's kernel work is paused (#3992), so its kernel bugs come here as regression cases:
one file per issue, `fixtures/solidcraft/<issue>-<slug>.json`. About an hour per case once
solidcore is built.
1. **Pick.** Open SolidCraft issue, reproduced by a triager, one-off, no PR or claim anywhere:
   `gh pr list -R tangency-labs/solidcore --state open --search "<number>"` and
   `ls fixtures/solidcraft | grep ^<number>`. The fixture format has no STEP import, so prefer
   clean primitives (revolve, cylinder, box, booleans) over a design with dozens of features; a
   failure that comes from error piling up over many features won't survive the reduction.
2. **Reproduce in SolidCraft** (headless MCP, see `repos/solidcraft.md`): open the reporter's
   file or rebuild the geometry from the issue's steps, and confirm the same error.
3. **Work out the expected answer** in closed form or by quadrature (Gauss-Legendre, checked on a
   grid) before running any kernel.
4. **Check solidcore on a fresh `main`.** Export the bodies as STEP (`file.export`), import with
   `step_brep::import`, run `boolean_difference` / `_union` / `_intersection`, print
   `brep_volume`. A scratch example under `crates/step-brep/examples/` does it; never commit it.
   Results can change between commits on the same day.
5. **Write the fixture** from a similar one: `issue`, `pr`, `test` (strings, required),
   `summary` (geometry, why it failed on truck, how the numbers were checked), `cases[]` of
   `steps` + `expect {lumps, volume {value, tol, formula}}`. Steps available: Box Sphere Cylinder
   Rotate Union Difference Intersection Extrude Revolve Pipe Loft Sweep Mirror Fillet OffsetFaces
   SheetCorner (difftest `Step` enum; check for new ones). Count lumps by reasoning about the
   shape, not by copying a kernel's answer.
6. **Run** `cargo test -p difftest --lib` first: the loader test requires `pr` to contain
   `/pull/`, so with no fixing PR point `pr` at the fixture PR itself and say so in `test`. Then
   `nice -n 10 cargo run -q -p difftest -- --solidcraft-cases '<issue>-*'`, then every case for
   regressions (a few minutes).
7. **PR.** Claim with an empty commit + draft PR carrying AGENTS.md section 4's scope block
   (Units / In scope / Expected edits / Evidence / Related PRs / Available follow-ups / Status).
   Title `difftest: SolidCraft #N case (...)`. Merge `origin/main` in; draft until the suite is
   green.

## Claim commit and "Run failed" emails
The empty `claim: #N` commit triggers a CI run that reports "Run failed ... No jobs were run" by
email. It is harmless (no code changed); judge by the real runs on the PR's later commits.
