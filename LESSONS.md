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
- **A triager's "reproduced" covers the failure, not the cases that seem to work.** Before
  designing a fix, measure the working cases against the right answer too. In one sketch-offset
  bug the report was a failure past a size threshold, but the offsets below it that succeeded were
  also joined at the wrong point, so a fix for the threshold alone would have kept wrong geometry
  and the real fix became a design question for the maintainers.
- **A claim draft is not the finished state.** Where a project claims issues by opening a draft
  PR, mark it ready as soon as the code and tests are done; leaving it a draft hides finished work
  from reviewers, and some projects' CI skips drafts entirely, so nothing has been checked
  upstream until it is ready.
- **A red check from a cancelled run is not a failure.** When a push or a label change starts a
  new CI run, the old one is cancelled and its summary check can stay red on the PR beside the new
  green one. Before reacting to a red check, find the newest run for the head commit and read that;
  only a failure in it counts.
