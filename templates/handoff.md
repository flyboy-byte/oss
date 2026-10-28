# Handoff: <project>. Status: <one line> (YYYY-MM-DD)

Written at the end of a session so a cold one (or another person) can pick up. Replace it each
time; history goes in the session log.

## Verify first (state here is a snapshot)
```sh
gh pr list -R <owner>/<repo> --author @me --state open
gh api notifications --jq '.[].subject.title'
git -C ~/<repo> status && git -C ~/<repo> log --oneline -3
df -h /home
```

## Open PRs
| PR | Branch | State | Waiting on |
| --- | --- | --- | --- |
| #N | fork:branch | draft / ready / REVIEW_REQUIRED / CI action_required | maintainer / me / CI |

## In progress
- What was being done, the attempts so far and their results, the last raw output.

## Next steps
1. The first concrete thing to do.

## Do not re-raise
- Decisions the owner already made, dead ends already checked, threads to leave alone.

## Paste-back (copy into a fresh session)
```
Resume <project> work. Read ~/projects/oss/CLAUDE.md, then projects/<name>/STATE.md and this
handoff. Verify with the commands under "Verify first" before trusting anything. Open: <PRs>.
Next: <step>. Do not re-raise: <list>. Public writes need my explicit OK each time.
```
