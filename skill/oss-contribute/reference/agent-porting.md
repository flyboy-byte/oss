# Porting a whole project with several agents, against a frozen original

Not a contribution to someone else's repo: this is for the owner's own fork, ported to another
language by a mix of agents (Claude, Codex and others) working in parallel over weeks. First used
2026-10-31 on a pure-Rust port of a browser measuring app (about 180,000 lines of TypeScript, kernels
included). Everything here was learned there; each rule exists because something went wrong or nearly did.

## The shape

1. **Freeze the original.** Pin one commit and tag it. The original stays runnable and is never edited
   (check that its tree has no diff against the tag after every merge). It is the judge, not a spec to
   be paraphrased.
2. **Be honest about provenance.** A port read from the source is a derivative work, not clean-room.
   Say so in the docs and keep every copyright and SPDX notice. Licensing, naming and any contact with
   the author are the owner's alone; park them in one file and do not start them.
3. **Break the work into units in JSON files**, one per file so agents do not conflict: id, lane,
   dependencies, status, evidence, open scope. Status goes
   `planned, specified, baseline-captured, implemented, compared, integrated, accepted`.
   Generated files (backlog, status) come from a script; never hand-merge them.
4. **Lanes set priority** (a small file ranks them), so a session told nothing can run one command
   (`next_task.py`) and take the first unclaimed unit. Change the order when an agent would otherwise
   pick work that is blocked.
5. **Claim with a draft pull request** titled with the unit id. Check open PRs first. One unit per PR
   from `main`. Files that many units touch (workflow files, the task runner, the suite list) belong to
   one open PR at a time.
6. **No handoff file is needed.** State lives in the repo: unit files, lanes, PRs, a roadmap. A fresh
   session reads those and runs the next-task command.

## Proving parity (the part that matters)

- **A green build is not parity.** The check that counts runs the original and the port on
  independent copies of the same input and compares success or failure, full error text, progress text
  and output bytes. Say in every report which of those were compared.
- **Skips are not passes.** A test that skips because a file is absent is reported as skipped, with the
  reason, never as green.
- **Write the cases first, from the original's own tests**, then port the whole body, then compare. Show
  the comparison fails on a deliberately wrong value in the port.
- **Never loosen a tolerance or a seed, and never regenerate expected values from the port.** Goldens
  come from the original.
- **Fixed cases miss hostile input.** Add seeded mutation fuzzing (header edits, truncation, byte flips,
  token swaps) against the original, run by someone who did not write the unit. In the first run about
  1,300 fixed cases passed while 282,000 fuzzed files found a real divergence (error text containing a
  JavaScript number rendered differently).
- **Measure what the comparison reaches.** Coverage of the original's branches and of the port's lines
  shows which behaviour is still unverified; that becomes the next corpus.
- **Implemented never means verified.** Give units a verification level (none, fixed corpus, fuzzed,
  coverage reviewed) and require the last for `accepted`.
- **Differences of limits are decisions, not bugs to hide.** Where the port refuses an input the
  original handles (a size cap, say), record it as a numbered finding and let the owner decide: keep,
  document or match. Do not tune a limit to make the report clean.

## Running a merge queue

- **The integrator reads the log of every red check.** One upstream wall-time test was a known flake;
  it was accepted only after reading the failing log each time, and its threshold was never raised.
- **Merge commits, never squash; bottom-up; retarget child PRs to `main`.** On a changelog conflict
  keep both entries. Regenerate generated files rather than merging them.
- **After each merge:** confirm CI on `main`, and confirm the original tree has no diff against the tag.
- **Merge only when the owner says** ("merge when green" counts, and so does pasting the agent's status).
  The sandbox may deny a merge or a bulk branch delete until the owner says so; retry once as written,
  then hand over the command. Do not reword around it.
- **Verify in a scratch clone, then delete it.** Not a worktree beside the repo. Symlink the missing
  dependency folder and remove the link before committing.

## Working with a second agent

- Findings from another model are input to check, not fact. Reproduce before accepting.
- Keep your own work off the other agent's files. Say in the PR body which units you touched.
- Give the other agent tasks that are specified and checkable. A spec written from the source
  (commands, errors, scheduling rules) with each rule tagged DOCUMENTED, INFERRED or DESIGN lets it
  implement against traces without re-reading everything.
- When an agent's first pick would be a blocked unit, fix the ranking, not the agent.

## Dependencies for missing capabilities

Prefer writing small, security-sensitive parts yourself. Allow a pure-Rust crate only after a recorded
decision covering licence, purity (no C or C++ underneath) and a behavioural check against the original.
In the first port the best image-decoder candidate passed licence and purity review and still failed
pixel parity against the browser (8 of 38 exact), so nothing was adopted.

## Tell the owner plainly how far along it is

Library ports and a test harness are not an application. Report units by status, say what cannot yet
be tried, and name the first milestone at which the owner can use it. No dates and no percentages.
