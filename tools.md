---
layout: page
title: Tools
permalink: /tools/
description: 'The tools Alexis Mas uses day to day — runtimes, terminal, editor, AI-assisted development, infrastructure and reference.'
updated: 2026-08-29
---

A running list of the tools I actually reach for, with a line on why each one
earned its place. This isn't a skills list — the [CV](/cv/) has that. These are
the smaller things: the ones I run every day, and the ones I'm glad to remember
exist on the day I need them.

## Environment and runtimes

- **[mise](https://mise.jdx.dev)** — one version manager for every runtime, and a task
  runner besides: the handful of commands a project needs, without a Makefile to keep
  them in.

## Terminal and shell

- **[Warp](https://www.warp.dev)** — command output split into blocks, which turns a
  two-hundred-line `terraform plan` into something you scroll back to rather than
  hunt for.

## Editor and AI-assisted development

- **[VS Code](https://code.visualstudio.com)** — took over from Atom years ago and
  hasn't been seriously challenged since. These days it's as much the thing
  devcontainers and agents attach to as the thing I type in.
- **[Claude Code](https://claude.com/claude-code)** — the agent itself; everything
  below is a way of running it.
- **[Conductor](https://conductor.build)** — runs several of them in parallel, each
  in its own git worktree, so unrelated tasks don't fight over one working tree.
  This page was built in one.
- **[Paseo](https://paseo.sh)** — what I'm trying out as Conductor's successor. A
  desktop client over a daemon that runs inside a devcontainer, so the containment
  below comes with the setup instead of being arranged per project.
- **[Dev Containers](https://containers.dev)** — the blast radius. An agent running
  unattended against a platform should be able to break its container and nothing
  else, which is what makes letting it run unattended a reasonable thing to do.

## Infrastructure and cloud

- **[dyff](https://github.com/homeport/dyff)** — a diff for YAML that reports each
  change by its path in the document rather than by line, so a re-indented or
  re-ordered manifest stops looking like a rewrite. Export it as
  `KUBECTL_EXTERNAL_DIFF` and `kubectl diff` shows you what actually changes before
  it lands.

## Reference and docs

- **[DevDocs](https://devdocs.io)** — quicker than hunting across five official doc sites.

{% comment %}
Uncomment a section once it has at least one entry, so the page never ships an
empty heading. Entry format: **[Name](url)** — one line on why it's here.

## Security and secrets

- **[Name](https://example.com)** — why it's here.
{% endcomment %}

*Last updated {{ page.updated | date: "%B %Y" }}.*
