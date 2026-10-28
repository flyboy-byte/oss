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
