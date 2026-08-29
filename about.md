---
layout: page
title: About
permalink: /about/
description: 'Alexis Mas — infrastructure and platform engineer in Valencia, Spain. Fifteen-plus years across mobile, Rails, API platforms and production Kubernetes.'
---

I'm an infrastructure and platform engineer. I've been building software professionally
since 2008, and for most of the last decade the thing I've been building is the ground other
engineers stand on.

## How I got here

I started on mobile and web — Android and Rails, mostly, for startups and agencies in
Spain and Uruguay, the later ones building for US clients. That's the era the
[archive](/archive/) is from, and it's why the older posts here are about Gradle and
RxJava.

The turn came at New Work SE in Germany, where I was the first engineer on a platform team
whose job was giving product teams a way to expose their APIs. I owned the existing Rails
REST gateway — 10k+ req/s, and it had to keep working — while designing and building its
GraphQL successor in Scala, learning both the language and the spec on the job. It ended up
adopted across the company, 100+ engineers. The team went from two people to about eight
while I was there.

That job taught me the thing I've organised my work around since: **a platform nobody adopts
is not a platform.** The gateway succeeded because we shipped features product teams actually
asked for, wrote a lot of documentation, ran onsite workshops across offices in Germany,
Spain and Portugal, and onboarded teams one at a time instead of all at once. I gave talks
about it at a meetup in Germany and a conference in Portugal, and recorded screencasts for
the internal blog to reach the people a workshop couldn't.

Then nearly six years at Wellfound, a US company, as one of two infrastructure engineers
supporting 20+ product engineers on a platform serving 3M+ monthly visitors. Reliability,
scalability and delivery pipelines, end to end: three production EKS clusters, Terraform
for everything underneath, GitOps with ArgoCD, on-call, incidents, cost, and a SOC 2
attestation. Owning that surface with two people means you get quite good at deciding what
not to do.

These days I work as a fractional platform engineer, contracting for US companies in
financial services. Most of that has been as the sole platform engineer on a market-data
platform — multi-terabyte Aurora, continuous ingestion from external data sources.
Building the observability, the IaC and the delivery story from a fairly bare starting
point.

## How I work

I've been fully remote for six years, in small autonomous teams that run async — all of it
with US companies, with colleagues on both coasts, which is where the async habits come from.
Valencia, Spain, on CET: full EMEA overlap, and afternoon overlap with the US East Coast.

I like the parts of infrastructure work that are really communication problems. Writing the
runbook. Naming the Terraform workspace so the blast radius is obvious from the outside.
Working out which of the fifty things in the dashboard is the one you'd actually want woken
up for. Infrastructure that only its author can operate isn't finished.

Lately most of my attention is on AI-assisted infrastructure work — development environments
where agents can run with a reduced blast radius, the feedback loops that make that safe, and
automating the operational toil that recurs often enough to be worth deleting.

## Elsewhere

Email me at [hello@alexismas.com](mailto:hello@alexismas.com). I'm on
[GitHub](https://github.com/Axxiss) and [LinkedIn](https://www.linkedin.com/in/alexis-mas).
The [CV](/cv/) has the detail.
