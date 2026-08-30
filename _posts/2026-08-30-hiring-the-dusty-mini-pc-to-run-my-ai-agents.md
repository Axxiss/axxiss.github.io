---
layout: post
title: "Hiring the dusty mini-PC to run my AI agents"
date: 2026-08-30
tags: [AI, Tooling]
---

I have been going deeper into agentic engineering for the past few months with the ultimate goal of understanding what the technology can do and how can I integrate that into my work i.e into a company while running a production system. For that I'm building what I call personalOS, an app that helps me managing my household, the small things of the day to day.

While doing so, I have caught myself staring at the screen waiting for the AI agent to finish or just peeking on what it is doing. Tapping the trackpad so there is some activity to prevent the laptop from sleeping. Walking towards the laptop just to see if the agent finished, while I'm not at the keyboard. As an anecdote it's fine, as a routine for a profesional, not so much.

So I set myself a goal, setup a system where agents look at my personalOS backlog and generate PRs overnight so I can review and correct the agents during my day. Ideally, learning something along the way which I can introduce on production systems.

I'm not starting from scratch, my setup evolved over the years, I'm currently starting from a semi-autonomous setup were I'm using Conductor to manage and control the agents and their worktrees. But as I wanted to run the agents on a devcontainer for a contained blast radius I switched to Paseo.

This is what my current setup looks like:

- project is isolated in a devcontainer
- the devcontainer contains (ba dum tss):
  - claude code
  - paseo daemon
  - all the tools needed to work on the project
- on the host: paseo desktop client

As I see it, the benefit is two fold. On one hand an AI agent running inside a devcontainer provide me with a controlled blast radius. All the agent see is what's inside the container. Meaning I have full control on what the agent sees: env vars, secrets, files, tools.

On the other, is that the client and the daemon are decoupled. The daemon running inside the devcontainer controls Claude Code while the client runs in another process and they talk to each other over HTTP. By using HTTP I can not just decouple them at the process level but at the instance i.e. running the daemon in one computer and the client in another. That's exactly what I did next. I setup a VPN with Tailscale installed Paseo on my phone and just like that I was able to control the agent running in my laptop.

Controlling the agent from my phone almost immediately made one problem evident: availability. Close the laptop lid, the agent goes to sleep. You have seen the picture (or are guilty yourself) of laptops with the lid ajar just to keep the agents grinding.

Nevertheless, that was one quick experiment that allowed to me tinker with personal projects. Which is great! As it's something I haven't done in years, pretty much since I became a father. Having the agent & client decoupled opened the door to move the agent's work from my laptop to another computer, solving another problem: power.

Power is a side effect of parallelism. When agents do their work they require energy to run tests, create artefacts, do lots of IO, etc; which if unplugged means draining the laptop's battery. One day it's fine, do it every day and the battery will notice it. So I'm forced to be plugged-in all day, beating the point of having a laptop. Furthermore, it's annoying, the laptop gets heated up, the fan is constantly working. My fingertips feel it.

With that in mind, I remembered that I have already a mini-pc at my office which I had bought for setting up home assistant at home. Surprise, surprise.... I never did. So it's there at my home office, gathering dust. It's not a super powerful piece of hardware. It's an Intel Celeron N5095 with 4 cores and 16 GB of RAM. Amazing huh? State of the art. Chances are the hardware falls short compared to my macbook pro, tests and builds will be slower but inference still runs on Claude's servers. Anyways, it should be more than enough for running an experiment.

I'm deliberately not going with a cloud based solution mainly because I don't want to add yet another service I need to pay for nor depend on.

Move the work to a machine that runs 24/7 and both problems are gone. I trigger an agent, close the laptop, and it keeps working while I walk to my favourite coffee shop. I can take the setup even further by scheduling work with things like:

- at 01:00 the agent goes through the codebase and updates outdated documentation
- from 02:00 to 06:00 agent looks at the backlog and implement the next items

So I wake up, make a coffee, and by the time I'm done with breakfast there's a bunch of PRs waiting for review. The agent worked overnight, I didn't. And I didn't stare anxiously at the screen while it was doing its thing.

Even before setting up the remote agent, a bunch of questions pop up:

- What did the agent do while I was not watching?
  - Which tools did it access?
  - Which URLs did it talk to?
  - Did it bypass any forbidden tool or command?

If we look at the problem through an organization lens, with multiple engineers or even multiple teams the questions take on another dimensions:

- Where are we running our remote agents?
- What are our fleet of agents doing?
- Which agent used tool X? When?
- What are our agents costing us?

That is what is what I want to explore next. But first, let's get the dusty mini-pc to work.
