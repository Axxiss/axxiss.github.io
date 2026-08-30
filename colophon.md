---
layout: page
title: Colophon
permalink: /colophon/
description: 'How this site is written and built — what Alexis Mas writes himself, what an AI agent drafts under his direction, and the stack underneath.'
---

A colophon is the note at the back of a book saying how it was made. A good part of
this site was drafted by an AI agent, so it seemed worth saying which part.

## The writing is mine

Everything under [/blog](/blog) is written by me — the argument, the words, and the
mistakes. That includes the older posts in the [archive](/archive/): they're from a
different decade of my career, they keep their original URLs, and they're mine too.
Posts are where I work out what I think, which is exactly the part I'm not
interested in handing over.

## The rest is drafted, and directed

The standing pages — [about](/about/), the [CV](/cv/), [tools](/tools/), and this
one — were drafted by an AI agent working from my notes and my direction. I decide
what each page is for, what belongs in it and what stays out. I read every line
before it ships and rewrite the ones that don't sound like me. The facts are mine
and I stand behind them; the first draft usually wasn't.

I don't think that split is a rule anyone else has to adopt, but it's where I've
landed. Assembling a page out of things I already know is a different job from
working out what I think, and it's one I'm happy to direct rather than type.

## How it's built

- **[Jekyll 4](https://jekyllrb.com)** — static files, no database and nothing to
  run in production. Markdown through kramdown, code through Rouge.
- **Hand-written CSS** — no framework and no build step. Colours live in one
  palette as named roles rather than as literal values scattered across the
  stylesheets, which is what makes the site re-themeable in an afternoon.
- **[GitHub Actions](https://github.com/features/actions)** — builds on every push
  to `master` and deploys to GitHub Pages. Source is on
  [GitHub](https://github.com/Axxiss/axxiss.github.io).
- **[html-proofer](https://github.com/gjtorikian/html-proofer)** — one command
  checks every internal link, image and anchor before anything ships. Cheaper than
  finding out from a reader.
- **[Claude Code](https://claude.com/claude-code)** in
  [Conductor](https://conductor.build) — the agent, running in its own git
  worktree, which is how a bad draft stays a discardable branch. The
  [tools](/tools/) page has more on that setup.
