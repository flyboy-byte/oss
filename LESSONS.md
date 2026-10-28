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
