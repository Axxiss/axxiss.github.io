---
layout: post
title: "The road into autonomous coding agents"
date: 2026-09-20
tags: [AI, Tooling]
description: "TBD — one sentence for search engines."
# Uncomment on publish, once the illustration exists. Left commented so
# bin/check-links passes on the draft — a missing image is a hard failure.
# image: /images/posts/TBD.png
# image_alt: "TBD"
---

I started my agentic development experiment from scratch, with a vanilla Claude Code: no skills, no plugins, no customization at all. My first two goals were quite clear: move the them to the mini-pc and get agents to work nights.

Moving it to the mini-pc was a big change, interacting with my agents either from my laptop or while on the go with my phone. Without worrying about agents going to sleep when my device does. With the first goal achieved, it was time to tackle the next one: schedule the night runs. To get those to work, agents would need be fully autonomous.

My workflow wasn't there yet, I was still on the loop, giving the initial instructions like saying what to implement, triggering `/simplify` and `/code-review`. Asking to fix the review's findings and what not until I was happy enough with the result to finally open a PR.

My first night run was a description of the workflow I was following:

1. Choose a bean
2. Implement the bean
3. Simplify the change
4. Review the change, and fix what the review finds (up to 3 times)

Once the nightly prompt proven to be right I extracted it to a skill which I called `/implement-a-bean`. Meaning the nightly prompt was just calling that skill.

Supringsingly, the nightly run is not what had the most impact on my workflow it was the skill. I've found myself transitioning from instructing agents to opening a few sessions, calling `/implement-a-bean` then move to something else and review the PRs aftwerwards. After a few days following the same repetitive behaviour the natural step was to remove myself out of the process. So I updated my Paseo schedule to run every 2 hours instead of 2 per night putthing a WIP limit to the amount of PRs open at any time.

```
1. Count the open pull requests.
2. If five or more are open, do nothing. Finish, and archive the workspace.
3. If fewer than five are open, run the `implement-a-bean` skill.
```

<br/>

A nice chart is worth more than a thousand word:

{% include charts/merges-per-day.html %}

Before setting the mini-pc up is where I put most of the effort, thinking through edge cases and plumbing things up. When I started with 2 PRs per night I was also running `/implement-a-bean` through out my day, it was a trigger and forget thing. Which explains the jump on the amount of PRs per day. Finally, I switched to agents running every 2 hours.

{% include charts/merges-cumulative.html %}

Goes without saying, that the scheduled agents result in shipping faster. Moving the bottleneck from build time to review time. Although if I'm being honest, I'm not reading the code on most of the PRS. I skim through the changed files looking for DB migrations otherwise I rely on the screenshots included on the PR description. One of the requirements for changes touching the UI is to include screenshots of the change. This allows me to gauge wether the implementation is what I wanted or not.

It wasn't perfect from the get go, while skimming through PRs I've observed that some of them were including more changes than originally planned. That was the agents going the extra mile: fixing related things or covering obscure edge cases. As a correction I created a rule so that one PR contains only one capability everything else must be a recorded as a followup bean. This had two consequences: reduce the long tail of big PRs (+1k additions) from ~6% to 2%. At the same time the agents started to contribute to the backlog by adding tickets for tackling technical debt or improving the product around and edge case.

{% include charts/followups-per-pr.html %}

All these changes had a visible impact on the output. Which were easy to measure: merged PRs, completed/created backlog items. But what about the small changes, like adding a paragraph to your AGENTS.md what's the impact of that in your pipeline? Can you measure it? Did the change moved the needle in the right direction?

In a month I've built a web to help me with the small recurring things that don't deserve their own app.
It has lists (plain and todo), care and subscriptions management; and time tracking with light/dark theme, household sharing and multi language support (en/es) without writing a single line of code. Yes, I did the intial DB modeling (but the AI implemented it) and made technical decissions at the boundary layer (how/where the app is deployed)

What's the quality of the codebase? does the code honors the defined architecture? is it secure? is the test suite any good?

As agents now write the code for me, my role on the project has changed. At the one hand I need to think at the product level, what features I want to include, what to prioritize, cleanup the backlog from dated items, etc. At the other, acting as a supervisor for my development pipeline ensuring all pieces work smoothly, reducing toil, removing friction so agent have the right context & tools to carry out their job.

Which brings many questions around what it will become of our profession.

Only time will tell... but not without digging.
