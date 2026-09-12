---
layout: post
title: "TBD"
date: 2026-09-14
tags: [AI, Tooling]
description: "TBD — one sentence, SEO."
---

After scavenging for cables in a box full of different type of cables found all I needed to setup the mini-pc at my desk and installed Debian Trixie.
Then I installed some basic tools for the setup I wanted (git, tailscale, ssh, docker) and created a bootstrap script to handle the provisioning of the mini-pc. Just a bunch of bash scripts in case I need to reprovision the mini-pc or move the workload to a new hardware. It's ugly but it works.

The setup from the first post moves onto the mini-pc mostly unchanged, just split across two machines. Each project is a devcontainer on the mini-PC, and inside it sits Claude Code, the Paseo daemon and the project’s tooling. The desktop client stays on both my Mac and on my phone. They talk over HTTP, making the decoupling possible. On each container the daemon listens on 6767 and dev servers get 4210-4219.

The migration didn't come without surprises, the SSH connection was slow to the point it was unusable. I could have spent days barking at the wrong tree, digging into networking or container configuration on my own. Thankfully, an agent doing more than I've asked diagnosed the issue as a hardware problem, which had an easy fix: plugging a cable into the mini-pc.

With the SSH problem gone, it was time to setup my personalOS project (which already had a devcontainer configured) into the mini-pc. Which immediately brought two questions.

How to manage the project? initial setup and interacting with the devcontainer (booting, restaring, runnings commands, reading logs, etc).

For that I ended up creating a small CLI tool which I called `fleet` (as if two containers were a fleet). It's a rather simple tool that run commands from my mac inside the mini-pc over SSH. Some of the commands it has:

```
  fleet add GIT_URL           # Clone a repo onto the host
  fleet exec REPO -- COMMAND  # Run one command in the devcontainer of a repo
  fleet up REPO               # Boot the devcontainer of a repo, and leave it running
```

The second question was: How to access the container?

Initially I was tempted to use the mini-pc as the access point for each container. Once I started to chew on the problem, I've realized the setup had two problems: port management and inferring which project I was interacting based on the port. Having to know which port mapped to which project was something not only I didn't wanted to experience but neither to manage. With one port per container could be doable but with plenty of them per container is a no go.

Instead, I added a Tailscale sidecard to each container. By doing so each container gets its own host (e.g. project-a.tailxxxx.ts.net). Meaning each
project exposes the same ports under their own host. So I could access `project-a:6767` and `project-b:6767`, immediately knowing which project I'm interacting with. Furthermore, it provided a consistent configuration accross projects.

Suddendly, the morning after the intial setup I couldn't SSH into the mini-pc anymore. It was the kind of failure we all hate, things breaking without anything changing. Turns out, by default Tailscale ask for re-authentication from time to time. Changing the policy form "check" to "accept" removed that step and by doing so some friction was gone.

The cost of not juggling with ports is the extra setup needed to get the Tailnet sidecar up and running. After a week of using it, it was the right choice. I know which project I'm interacting with by just glimping at the URL both on Paseo's config or on my browser.

Summing up, devcontainers in combination with a tailscale sidecar allowed me to run autonmous during the past week. Since then my agent are working nights, I have them scheduled on Paseo to run at 01:00 and 03:00 am, implementing a PR on each run. Which I review and merge while enjoying a coffee in the morning.
