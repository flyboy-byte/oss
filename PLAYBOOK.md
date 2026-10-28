# Playbook: turning a SolidCraft kernel bug into a solidcore regression fixture

Proven on #5794 and #7773 (2026-10-27). Takes about an hour per case once solidcore is built.

1. **Pick the issue.** Open, a triager has reproduced it, one-off, no PR or claim anywhere. Check: `gh pr list -R tangency-labs/solidcore --state open --search "<number>"` and `ls fixtures/solidcraft | grep ^<number>`. Prefer clean primitives (revolve, cylinder, box, booleans) over designs with dozens of features: the fixture format has no STEP import.
2. **Reproduce in SolidCraft (headless MCP).** Open the reporter's file if the MCP's version reads its format (`solidcraft-cli --version`; format 40 needs 0.24.1+; copy the file into the MCP's allowed /tmp/solidcraft-mcp-*/ folder). Otherwise rebuild the geometry from the issue's steps. Confirm the same error.
3. **Know the right answer before trusting any kernel.** Compute the expected volume from closed form or a quadrature (Gauss-Legendre) and cross-check on a grid. #5794: 20019.93 - 10.003*7*17.5 = 18794.56. #7773: tube pi*(28.8^2*20 - (20^3-4^3)/2.4); overlap = integral of circular-segment areas.
4. **Check solidcore itself.** Export the bodies as STEP (`file.export`, `bodies: [...]`), import with `step_brep::import`, run `boolean_difference` / `boolean_union` / `boolean_intersection`, print `brep_volume`. A scratch example under crates/step-brep/examples/ does it (never committed). Pull `main` first: results changed between two commits on the same day.
5. **Write the fixture** from the template of a similar existing one (e.g. 3303 for cone + cylinder): `issue`, `pr`, `test` (strings, required on main), `summary` (geometry, why it failed on truck, how the numbers were checked), `cases[]` of `steps` + `expect {lumps, volume {value, tol 0.01, formula}}`. Count the lumps by thinking through the shape, not by copying the kernel's answer (#7773: the "prism minus tube" really is 2 lumps; my first guess of 1 was wrong).
6. **Run.** First `cargo test -p difftest --lib` (the loader test requires `pr` to contain /pull/; for an issue with no fixing PR, point `pr` at the fixture PR itself and say so in `test`). Then `nice -n 10 cargo run -q -p difftest -- --solidcraft-cases '<issue>-*'`, then the whole suite for regressions (131 cases, a few minutes).
7. **PR.** Claim first: empty commit + draft PR with the scope block from AGENTS.md section 4 (Units / In scope / Expected edits / Evidence / Related PRs / Available follow-ups / Status). Title `difftest: SolidCraft #N case (...)`. Merge origin/main into the branch (no rebase). Leave draft until the suite is green.

## Tool notes
- solidcore builds into `bin/`, not `target/`. Delete build output with `cargo xtask rm`, not `rm -rf`.
- Clippy needs `-A clippy::assert_is_empty -A clippy::explicit_iter_loop -A deprecated -A clippy::mut_mut` on a newer toolchain than CI's pinned 1.98.
- The headless MCP's direction names: "positive" from a body face can point away from the body; check the bbox before believing a cut.
