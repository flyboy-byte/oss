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
- **The maintainer side: running your own project that people and their agents send PRs to.** The skill
  is written for the contributor. On the owner's own public repo the same rules turn around:
  - One authoritative agent-rules file (`AGENTS.md`) that any model reads, with `CLAUDE.md` as a pointer
    to it. Rules for PRs are "awareness, not rules": the PR template asks for what and why, pros, cons
    and risks, *what the other maintainer might think*, how it was tested (on a device, CI only, not at
    all), and how to revert. Agents write their own PRs that way and never gate someone else's PR on a
    missing section.
  - PR titles feed the changelog and the in-app release notes, so they are typed (`fix(server): ...`)
    and written for the person using the app, not for the code.
  - Three issue forms (bug, feature request, feedback) with blank issues off and labels to match. The
    app opens the right form pre-filled; for bugs it offers a numbers-only summary (versions, counts, no
    names or sites), and every form says never to paste logs or screenshots into a public tracker.
  - Big choices go into a numbered decisions log with who decided and the date, so a co-maintainer's
    agent can read the reason instead of re-asking.
  (First tried 2026-11-23 to 2026-11-25 on one two-maintainer Android app with a self-hosted server;
  outcome pending: no outside contributor has used it yet.)
- **Taking a private repo public without rewriting history.** Clean the current files first (server
  names, personal addresses, recovery artifacts), write down what old commits still show and rate the
  risk, delete any secret artifact before the visibility flips, then decide on a rewrite. A rewrite
  changes every hash, breaks changelog links and existing clones, and old PR refs still need the host's
  support to purge, so "what visitors see is clean" can be the better answer. (First tried 2026-11-24.)
- **Relicensing because a dependency's licence demands it.** A model's weights carry a licence like
  code, and a converted copy (another file format, another precision) is still that work. When a
  model the app needs was AGPL-3.0, the project moved to AGPL-3.0: every copyright holder agreed
  first, the agreement was written into the decisions log with its date, the model's licence text was
  shipped next to it, and the conversion script was committed so the file can be rebuilt. Check the
  licence of models, datasets and block lists as well as libraries. (First tried 2026-11-25.)
- **A compatibility window that CI enforces, for apps that talk to servers other people run.** Give the
  wire protocol a level and a window (the server serves this level and the two before it); send the
  level as a header both ways so each side can say which one to update; freeze one test suite per
  level that is never edited afterwards, only added to; and run a check in the release workflow that
  fails when the level changes without its suite. It turns "a contributor's change silently breaks
  every self-hoster on last month's release" into a red check. (First tried 2026-11-25; outcome
  pending: the level has not been raised yet.)
