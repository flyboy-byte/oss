# Playbook: workflows being proven

Repeatable workflows from real contributions, kept here while they are still being tried. Once one
has worked on more than one case it moves into the skill and leaves a pointer here.

## Moved into the skill
- ~~SolidCraft kernel bug to solidcore regression fixture~~: now in
  [`skill/oss-contribute/repos/solidcore.md`](skill/oss-contribute/repos/solidcore.md), "SolidCraft
  kernel bugs as fixtures" (moved 2026-10-28; proven on SolidCraft #5794 and #7773). The tool notes
  (build dir, `cargo xtask rm`, clippy allows, MCP direction names) are in the same profile and in
  `repos/solidcraft.md`.

## Being tried
- **A likely-related report gets an offer, not a duplicate call.** When another open issue looks
  like the bug your PR fixes but has no reproduction, comment that it may be the same failure, name
  the PR, and ask for the file so you can check it against the fix. Then test it before claiming
  the PR fixes it. (First tried 2026-10-28; outcome pending.)
- **Scaffolding a large port from a handoff packet.** Unpack an archive into a scratch directory
  first and read its listing; run its own integrity and plan checks; place it in the repo; confirm
  the pinned baseline with `git rev-parse HEAD`; branch locally. Build the first gate before any
  feature (format, lints, tests, the wasm check, plan checks, a pin-consistency check) and show it
  can fail by breaking one input on purpose. Record each departure from the packet as a numbered
  decision or finding file instead of editing the packet's prose, and write the contributor
  files (agent rules, PR and issue templates, a CI workflow that says plainly it is not the
  parity gate). Add a short ordered guide for a non-expert reader that tells the story with dates.
  (First tried 2026-10-31; outcome pending.)
- **Porting a whole fork with several agents against a frozen original.** Pin and tag the original and
  never edit it; keep work as one JSON unit file per task with lanes for priority and a next-task
  command; claim by draft PR; compare original and port on independent copies of the same input; fuzz
  by someone who did not write the unit; merge with merge commits, bottom-up, only when the owner says.
  Full write-up: [`skill/oss-contribute/reference/agent-porting.md`](skill/oss-contribute/reference/agent-porting.md).
  (First tried 2026-10-31 to 2026-11-02; 44 PRs merged, no unit accepted yet; outcome pending.)
