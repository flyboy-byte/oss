# storytold/cadcraft

Pure-Rust AutoCAD-class CAD app (layers L0-L5, egui UI, WASM). Agent-driven: contributions by AI agents are expected. Remotes: `origin` = storytold (read-only), `fork` = your fork. Profile: `cadcraft.env`.
Read the repo's `CLAUDE.md` each time.

## Pick and claim
- Work list: `docs/gaps.md` (pick from its top rows); `plan/` is gitignored and absent in a clone.
- No claim commits or draft PRs. Search open+merged PRs for the gap id (e.g. `F5`, `U10`) and the
  command name before starting, and again before pushing. A duplicate PR that is better wins:
  close ours and say so.

## Non-negotiables (from the repo's CLAUDE.md)
Clean-room (never read GPL CAD sources, never copy Autodesk assets); no `unwrap/expect/panic`
in non-test code; input-derived numbers are hostile (cap sizes, checked arithmetic); every
user-visible change is a command in `crates/engine/src/cmd/*` with tests; never break wasm.

## CI to mirror
- `CARGO_PROFILE_DEV_DEBUG=0 cargo xtask ci` (fmt, clippy -D warnings, test, assets, layers,
  wasm). Run it in the background; it takes a while. Debug info off keeps `target/` manageable.
- Workflow edits: `actionlint` (Packaging lint runs actionlint + shellcheck on every PR).
- Upstream workflows wait for maintainer approval. To see all five (CI, Packaging lint, Nix,
  FreeBSD, Windows ARM64) early, open a throwaway PR inside the fork (base `<you>:main`),
  read every run, then close it.
- In-app checks: rebuild the binary first, drive it with `--control PORT` JSON lines
  (`cmdline.input`, `engine.execute`, `ui.screenshot`), kill it by exact PID.

## Sync
Rebase on `origin/main`, push `--force-with-lease` to `fork`. Exception: don't rebase a PR whose
approvals are tied to its head unless it conflicts. `docs/gaps.md` (status line + revision table)
is edited by nearly every PR and is the conflict hotspot: keep upstream's rows, put ours on top,
bump the date. When one of our PRs merges, the others usually need this rebase.

## PR body
Match merged #799: prose explaining the gap and what changed, then **Checked:**, **Tests:**,
**Not covered:** paragraphs; no headings. Mention the `docs/gaps.md` row updated.

## Review
- CodeRabbit reviews every PR with **inline** comments: read `gh api
  repos/storytold/cadcraft/pulls/N/comments`, verify each finding, fix the valid ones, reply in
  the thread; it resolves its own threads after a fix.
- FredApps reviews and approves but has no write access, so `reviewDecision` stays empty even
  when approved. Treat their approval as an approval for the head rule.


---

# From the project's own instructions (refined copies, kept separate)

storytold has no `skills/` folder in cadcraft or in the sibling repos checked (photocraft, pdfcraft,
vectorcraft, artcraft, craft-libs, craft-contrib). Its guidance is `AGENTS.md` (`CLAUDE.md` holds the
same text), read 2026-10-29 at cadcraft `main` 336243e0 (Apache-2.0 / MIT). It points at a shared
`storytold/craftrules` repo for standards (never-crash, progress-docs); that repo is not public
(404), so those standards are unread. Nothing above this line was changed. **new** = not in our
notes; **same** = already there.

- **new:** contributor credits. Check `contributors/people.toml` for a `[people.<username>]` entry
  for the contributor. If there is none, ask them once whether they want a real name or display name
  shown (About window); if yes, add only their own entry in their PR, if no change nothing. Never
  add, edit or guess anyone else's entry; never hand-edit `contributors.json` (generated).
- **new:** more of the never-crash standard: bound recursion (nested or cyclic block references,
  `MAX_BLOCK_DEPTH`); every step of an interactive command runs under `catch_unwind`
  (`crates/engine/src/guard.rs`); `hostile_dxf_does_not_panic` next to `hostile_params_never_panic`.
- **new:** time goes through `chrono`; never hand-roll calendar or UTC conversion.
- **new:** fonts live in `storytold/craft-fonts` and test corpora in `storytold/<app>-corpus`, never
  in the repo; code and tests must work without them. Don't commit large binary fixtures.
- **new:** `engine.execute` never opens a dialog; only menu or toolbar starts the interactive
  prompt. `cmdline.input` types at the command line like a user.
- **new:** headless pictures: `cadcraft-cli run --sample --script 'LINE 0,0 10,10\n' --export out.png`;
  `cadcraft-cli info file.dxf`; `ui.render` when the screen is locked and no frame is presented.
- **new:** parallel agents use separate `CARGO_TARGET_DIR`s and delete theirs when done; `mv` and
  `cp` may be aliased interactive, use `/bin/mv -f` and `/bin/cp -f`.
- **new, a difference to remember:** the maintainers observe the reference app black-box on their own
  machine; a contributor has no such access, so use public specs only (DXF Reference, ODA's DWG spec).
- **same:** clean-room, asset rows in ATTRIBUTION.md, layering, `cargo xtask ci`, work list in
  `docs/gaps.md`.
- **Not borrowed:** "commit and push to `main`", the `plan/` session protocol and "don't stop to ask":
  those are the maintainers' own workflow, not a contributor's.
