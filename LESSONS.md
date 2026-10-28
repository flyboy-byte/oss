# Lessons (mistakes made, so the next session doesn't repeat them)

- **Check which kernel a project uses before diagnosing.** SolidCraft's kernel is truck, not solidcore; I once said a fix belonged in solidcore.
- **A "no result" cut can be a wrong-direction cut.** On #5794 I briefly concluded the cut was a no-op because the tool sat outside the body; the direction label was the cause. Measure the bbox and the removed volume first.
- **Re-test on the current main.** solidcore failed #5794's cut on one commit and passed it a few commits later; an old result is not a finding.
- **The expected answer must come from math, not from a kernel.** Both of my wrong turns were caught by comparing with a closed-form volume.
- **Don't draft claims from a stale snapshot.** plan/STATUS.md was weeks behind main; audit units from plan/units/*.toml, not the summary.
- **Licence is not just "is it open source".** SolidCraft is MIT OR Apache-2.0; another project's reciprocal OSL-3.0 code may be read for ideas, not copied.
- **Public writes need explicit OK.** Posting to GitHub was blocked by the harness once when the instruction was ambiguous; Logan's explicit "post this" fixed it. Ask, then post.
- **Large tool dumps waste context.** `describe_command`, `body_topology` and unit-toml `notes` fields are huge; use narrow queries.
- **zsh:** quote `=====` and globs (`--include='*.rs'`).
- **Disk:** check `df -h /home` first; solidcore's `bin/` is 40 GB. Cleaning is the owner's call.
- **Run CI's own unit tests before marking a PR ready, not only the feature run.** #1373 (a fixture-only PR) failed CI's `cargo test --workspace` because `solidcraft::tests::every_case_file_loads` requires every fixture's `pr` field to contain `/pull/`; I had validated with `difftest --solidcraft-cases` and the suite but not `cargo test -p difftest --lib` (2.5 s). Always read the PR's CI test command and run the matching crate tests.
