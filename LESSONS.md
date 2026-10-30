# Lessons (mistakes made, so the next session doesn't repeat them)

The inbox for the skill. A lesson lands here first; once it is written into the skill it is struck
through with where it went, not deleted.

- ~~**Check which kernel a project uses before diagnosing.**~~ SKILL.md section 1, "Which component
  owns the behaviour". (SolidCraft's kernel is truck, not solidcore; I once said a fix belonged in
  solidcore.)
- ~~**A "no result" cut can be a wrong-direction cut.**~~ SKILL.md section 3, "Measure before
  concluding nothing happened". (#5794: the direction label was the cause.)
- ~~**Re-test on the current main.**~~ SKILL.md section 3, "Reproduce first, on a fresh main".
- ~~**The expected answer must come from math, not from a kernel.**~~ SKILL.md section 3, "Know the
  right answer independently".
- ~~**Don't draft claims from a stale snapshot.**~~ SKILL.md section 1, "Status and plan summaries
  lag".
- ~~**Licence is not just "is it open source".**~~ SKILL.md section 1, "Licence of anything you
  read".
- ~~**Public writes need explicit OK.**~~ SKILL.md, after First run (2026-10-28): the harness blocks
  ambiguous ones; ask, or give the `!` command.
- ~~**Large tool dumps waste context.**~~ SKILL.md "Git habits", "Ask tools for narrow answers".
- ~~**Disk: check `df -h /home` first.**~~ SKILL.md "Git habits" and "Resuming".
- ~~**Run CI's own unit tests before marking a PR ready.**~~ SKILL.md section 4, "CI's unit tests
  too, even for a data-only change" (solidcore #1373 failed `every_case_file_loads`).
- ~~**Fork contributors can't re-run CI; read failed logs narrowly.**~~ SKILL.md section 5
  (2026-10-28, from #1373's second run).
- **zsh:** quote `=====` and globs (`--include='*.rs'`). Not in the skill: shell-specific.
- ~~**A triager's "reproduced" covers the failure, not the cases that seem to work.**~~ SKILL.md
  section 3, "Know the right answer independently" (folded 2026-10-29).
- ~~**A claim draft is not the finished state.**~~ SKILL.md "Pace: draft until the evidence is in"
  (folded 2026-10-29, which also replaces the "an hour after claiming is a warning sign" line).
- ~~**A red check from a cancelled run is not a failure.**~~ SKILL.md section 5 (folded 2026-10-29).
- ~~**An open issue is not an open bug.**~~ SKILL.md section 2, "Read the whole thread before you
  pick" (2026-10-29: of four label-search picks, two were already fixed, one by design, one
  claimed).
- ~~**A failing test is not yet a reproduction.**~~ SKILL.md section 3, "Check the repro hits the
  case you mean" (2026-10-29: a wrong sign on a sketch plane's second axis made the first test fail
  for the wrong reason; printing the distance showed it).
- ~~**Stop at the diagnosis when the fix is in a paused or foreign area.**~~ SKILL.md section 3,
  "When the cause is in an area you may not change" (2026-10-29).
- ~~**An installed build is not `main`.**~~ SKILL.md section 3, "Reproduce first" (2026-10-29: app
  screenshots from an older installed release were taken for current behaviour).
- ~~**A blocked write plus an explicit "do it".**~~ SKILL.md "First run" (2026-10-29): retry once as
  written, then give the command; never reword to get past a block.
- ~~**A PR can be queued without warning.**~~ SKILL.md section 6, head rule (2026-10-29).
- ~~**Parallel worktrees multiply incremental build output.**~~ SKILL.md "Git habits" and
  `repos/solidcraft.md` (2026-10-29: 26 GB regrown within a day).
- **A new test can pass on untouched `main`.** In a UI search fix, the first test used a helper
  that finds a control whether or not it is drawn (hidden controls sit off screen but still
  exist), so it passed with no fix at all, and again with half the fix removed. Run the new test on
  unmodified `main` first and watch it fail with the reporter's message; assert on what the user
  sees (drawn, on screen), not on what exists; then take the fix apart one piece at a time and see
  a failure each time. (2026-10-30, solidcraft #9318.) Candidate for SKILL.md section 3, "Prove the
  tests".
- **Issues found by someone else's automation are candidates, not picks.** A list from another tool
  or session went through the usual screen: a label like `kernel-paused` or an area tag drops an
  issue in one read, a claim comment or an open PR that names the number drops another, and what
  is left still gets the whole-thread read and a fresh-`main` repro. Of five listed, one was left.
  (2026-10-30, solidcraft.)
- **A long build belongs in the background.** A first build in a cold target took 18 minutes and
  blocked the foreground at the 10-minute limit. Start long builds and test runs in the background
  with output to a file, record the saved patch and the exact restore commands on the NOW board
  before a step that swaps files in and out, and take the next step when the completion notice
  arrives. (2026-10-30.)
