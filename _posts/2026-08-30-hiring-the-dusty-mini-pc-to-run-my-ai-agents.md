---
layout: post
title: "Hiring the dusty mini-PC to run my AI agents"
date: 2026-08-30
tags: [AI, Tooling]
description: "Why I'm moving my AI coding agents off the laptop: devcontainer isolation, a daemon and client that can run on different machines, and a dusty Intel mini-PC that stays awake all night."
---

I have been going deeper into agentic engineering for the past few months. The ultimate goal is to understand what the technology can do, and how I can integrate it into my work: into a company running a production system. For that I'm building what I call personalOS, an app that helps me manage my household, the small things of the day to day.

While doing so, I have caught myself staring at the screen waiting for the AI agent to finish or just peeking at what it is doing. Tapping the trackpad so there is some activity to prevent the laptop from sleeping. Walking towards the laptop just to see if the agent finished. As an anecdote it's fine, as a routine for a professional, not so much.

So I set myself a goal, set up a system where agents look at my personalOS backlog and generate PRs overnight so I can review and correct the agents during my day. Ideally, learning something along the way which I can introduce into production systems.

I'm not starting from scratch, my setup evolved over the years, I'm currently on a semi-autonomous setup where I'm using [Conductor](https://conductor.build) to manage and control the agents. Conductor gives each agent its own git worktree, so two agents never fight over the same files. But the agents run on my Mac, as me, with my permissions. That's isolation between agents, not from my machine, something Conductor is [upfront about in its docs](https://www.conductor.build/docs/concepts/workspaces-and-branches#what-isolation-gives-you): "development isolation, not a security boundary". So I wanted the agents in a [devcontainer](https://containers.dev) for a contained blast radius, and switched to [Paseo](https://paseo.sh), which puts the agent inside the container by default.

This is what my current setup looks like:

- project is isolated in a devcontainer
- the devcontainer contains (ba dum tss):
  - [Claude Code](https://claude.com/claude-code)
  - Paseo daemon
  - all the tools needed to work on the project
- on the host: Paseo desktop client

As I see it, the benefit is twofold. On one hand an AI agent running inside a devcontainer provides me with a controlled blast radius. As far as the running system goes, all the agent sees is what's inside the container. Meaning I have full control over what goes in there: env vars, secrets, files, tools.

On the other, the client and the daemon are decoupled. The daemon running inside the devcontainer controls Claude Code while the client runs in another process and they talk to each other over HTTP. By using HTTP I can decouple them not just at the process level, but at the instance level, i.e. running the daemon on one computer and the client on another. That's exactly what I did next. I set up a VPN with [Tailscale](https://tailscale.com), installed Paseo on my phone and just like that I was able to control the agent running on my laptop.

Controlling the agent from my phone almost immediately made one problem evident: availability. Close the laptop lid, the agent goes to sleep. You have seen the picture (or are guilty yourself) of laptops with the lid ajar just to keep the agents grinding.

Still, it was a quick experiment that got me tinkering with personal projects again. Which is great! It's something I haven't done in years, pretty much since I became a father.

Having the agent and client decoupled also opened the door to moving the agent's work from my laptop to another computer, solving another problem: power.

Power is a side effect of parallelism. When agents do their work they require energy to run tests, create artefacts, do lots of IO, etc. Which, if I'm unplugged, means draining the laptop's battery. One day it's fine, do it every day and the battery will notice it. So I'm forced to be plugged-in all day, defeating the point of having a laptop. And it's annoying, the laptop gets heated up, the fan is constantly working. My fingertips feel it.

I'm deliberately not going with a cloud-based solution. I don't want yet another service to pay for, another vendor to be locked into, and another place where my data could leak.

With that in mind, I remembered that I already had a mini-PC at my home office which I had bought for setting up [Home Assistant](https://www.home-assistant.io) at home. Surprise, surprise... I never did. So it's there, gathering dust. It's not a super powerful piece of hardware. It's an Intel Celeron N5095 with 4 cores and 16 GB of RAM. Amazing, huh? State of the art. Chances are the hardware falls short of my MacBook Pro, tests and builds will be slower but inference still runs on Claude's servers. Anyways, it should be more than enough for running an experiment.

Move the work to a machine that runs 24/7 and both problems are gone. It never sleeps, and it's not running off my battery. That's what I'm aiming for: I trigger an agent, close the laptop, and it keeps working while I take my daughter to school. From there it could go further, with scheduled work like:

- at 01:00 the agent goes through the codebase and updates outdated documentation
- from 02:00 to 06:00 the agent looks at the backlog and implements the next items

So the morning looks like this: I wake up, make a coffee, and by the time I'm done with breakfast there's a bunch of PRs waiting for review. The agent worked overnight, I didn't. And I didn't stare anxiously at the screen while it was doing its thing.

Even before setting up the remote agent, a bunch of questions pop up:

- What did the agent do while I was not watching?
  - Which tools did it access?
  - Which URLs did it talk to?
  - Did it bypass any forbidden tool or command?

If we look at this through an organisational lens, with multiple engineers or even multiple teams the questions take on another dimension:

- Where are we running our remote agents?
- What is our fleet of agents doing?
- Which agent used tool X? When?
- What are our agents costing us?

That is what I want to explore next, once the dusty mini-PC has actually started the job.
