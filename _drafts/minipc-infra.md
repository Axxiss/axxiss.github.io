---
layout: post
title: "TBD"
date: 2026-09-14
tags: [AI, Tooling]
description: "TBD — one sentence, SEO."
---

After scavenging for cables in a box full of HDMI and USB cables; and a few chargers as well I found all I needed to setup the mini-pc at my desk, plugged in an old keyboard, booted it up to see it has Ubuntu 24 installed. Surprisingly, I remembered the password so I logged in and proceeded to install Debian Trixie.

Once Debian was in place, I installed some basic tools for the setup I wanted (git, tailscale, ssh, docker) and created a bootstrap script to handle the provisioning of the mini-pc. Nothing crazy, just a bunch of bash scripts in case I need to reprovision the mini-pc or move teh workload to a new hardware. It's ugly but it works.

The setup process didn't come without surprises, I've already shared about the SSH connection being slow and how an AI agent diagnosed the issue. But once that was solved it went quite smoothly.

With all the building blocks in place it was time to setup my personalOS project (which already had a devcontainer configured) into the mini-pc. Which immediately brought two questions.

How to manage the project? initial setup and interacting with the devcontainer (booting, restaring, runnings commands, reading logs, etc).

For that I ended up creating a small CLI tool which I called `fleet` (as if two containers were a fleet). It's a rather simple tool that run commands from my mac inside the mini-pc over SSH. This is the help output:

```
fleet help
Commands:
  fleet add GIT_URL           # Clone a repo onto the host
  fleet completion SHELL      # Install the shell completion, and a `fleet` on your PATH
  fleet doctor                # Check each precondition, and name the fix for each failure
  fleet down REPO             # Stop the devcontainer of a repo, and keep the clone
  fleet exec REPO -- COMMAND  # Run one command in the devcontainer of a repo
  fleet help [COMMAND]        # Describe available commands or one specific command
  fleet logs REPO             # Read what a repo's devcontainer printed
  fleet ls                    # List every repo on the host
  fleet restart REPO          # Stop the devcontainer of a repo, and start it again
  fleet rm REPO               # Remove the containers of a repo, then delete its clone
  fleet shell REPO            # Open an interactive shell in the devcontainer of a repo
  fleet status                # Report facts about the host
  fleet up REPO               # Boot the devcontainer of a repo, and leave it running
```

The second question was: How to access the container?

Initially I was tempted to use the mini-pc as the access point for each container. Realizing immedialty it would turn into a nightmare rather quickly. As each container needs to expose their paseo daemon and the dev server ports. Instead, I added a tailscale sidecard to each container. By doing so each container gets its own host (e.g. axxiss-foo.tailxxxx.ts.net). Meaning I can access both `axxiss-foo.tailxxxx.ts.net` for Paseo managing the daemon and `axxiss-foo.tailxxxx.ts.net:3000` making super useful for accessing the dev servers from my browser.

That is the setup that allowed me to run autonmous agents during the past week. Every night my agents implement two PRs which I review and merge while I dring a coffee in the morning.
