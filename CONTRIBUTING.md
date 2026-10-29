# Contributing

**Anything is welcome.** This repo is a working notebook for contributing to other people's
open-source projects with an AI agent, and the notebook gets better when more people write in it.
You do not need to be an expert, to use Claude Code, or to have merged a PR anywhere.

## Ways to help

| You have... | Do this | Where it goes |
|---|---|---|
| A mistake your agent made (or you made) and what fixed it | Open a PR adding it | [`LESSONS.md`](LESSONS.md) |
| A workflow that worked more than once | Open a PR adding it under "Being tried" | [`PLAYBOOK.md`](PLAYBOOK.md) |
| A project you contribute to | Add a profile: AI policy, how work is claimed, CI commands, gotchas | [`skill/oss-contribute/repos/`](skill/oss-contribute/repos/) (start from `_template.md`) |
| A step in the skill that is wrong, unclear or missing | Open a PR, or an issue if you are unsure | [`skill/oss-contribute/`](skill/oss-contribute/) |
| A better template, or a README paragraph that confused you | Open a PR | [`templates/`](templates/), [`README.md`](README.md) |
| A question, an idea, or "this didn't work for me" | Open an issue | Issues |
| Use with another agent or editor (not Claude Code) | Tell us what you had to change | Issue or PR |

Half-finished is fine. A rough lesson in a draft PR, or an issue that just says "this step lost me
an hour", is useful. We will help shape it.

## What makes a change land

Same bar the skill sets for contributing upstream, applied here:

1. **Say what went wrong or what you saw.** One real incident beats a general opinion. "The agent
   pushed after the approval and the PR left the merge queue" is a lesson; "be careful with pushes"
   is not.
2. **Small.** One topic per PR. Do not reformat files you did not need to touch.
3. **Honest.** Say what you ran and what you did not. Never claim a check you did not do.
4. **Generic.** No names of people, private chats, tokens, emails or paths from your machine. Use
   the noreply address for your commits (`git config user.email "<id>+<login>@users.noreply.github.com"`).
5. **Not a secret.** Do not paste your own `~/.config/oss-contribute/` files. They are personal and
   stay outside this repo.

## A note on changes to the skill

[`SKILL.md`](skill/oss-contribute/SKILL.md) tells an agent what to do, so a change to it changes
how everyone's agent behaves. Those PRs get read line by line, and the maintainer merges them.
That is not distrust of you; it is how a shared instruction file stays safe. It helps if the PR
says which lesson or incident the change comes from. Lessons and playbook entries are lighter:
they are notes for people, and they are how things graduate into the skill.

## How a PR here works

1. Fork, branch, edit. There is nothing to build.
2. Open a PR with the template. Draft is fine.
3. A maintainer reads it, may ask for a rewrite to make it more general, and merges. The same
   rule applies to us: if it is slow, a polite nudge after a week is fine.

AI-written PRs are welcome here, since this repo is about working with AI. Please say so in the PR
(a "Generated with Claude Code" line is enough) and read the result yourself before you open it:
you are the author of record.

## Be kind

Be considerate, assume good faith, and critique the text, not the person. Disagreements about what
the skill should say are exactly the point of the repo; make them with evidence.
