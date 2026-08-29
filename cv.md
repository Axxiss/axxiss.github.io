---
layout: page
title: CV
permalink: /cv/
description: 'CV of Alexis Mas — Senior Infrastructure Engineer. Kubernetes (EKS), AWS, Terraform, Pulumi, ArgoCD, Datadog, incident response and developer tooling.'
---

**Senior Infrastructure Engineer** · Valencia, Spain · Remote (CET, full EMEA overlap)

[hello@alexismas.com](mailto:hello@alexismas.com) ·
[LinkedIn](https://www.linkedin.com/in/alexis-mas) ·
[GitHub](https://github.com/Axxiss)

Infrastructure and platform engineer with 15+ years of experience running production
Kubernetes (EKS) and AWS for high-traffic products. I own reliability end-to-end (Terraform
and Pulumi, GitOps delivery, observability, on-call and incident response, cloud cost), and I
build internal developer tooling as a product: requirements gathered from the engineers who
use it, then documented and driven to adoption through workshops, written guides, screencasts
and conference talks.

Six years fully remote in small, autonomous, async teams, most recently as the sole platform
engineer on a multi-terabyte data platform. Currently focused on AI-assisted infrastructure
work: agent-ready development environments, tight feedback loops, and automating recurring
operational toil.

## Experience

### Coinwatch — Senior Software Engineer, Infrastructure

*Nov 2025 – Present · Remote (Spain) · Contract*

AWS (ECS, EC2, Aurora/RDS, S3, ECR, SSM, VPC) · Pulumi (TypeScript) · Datadog · Ruby on Rails
· Sidekiq · Docker · CodePipeline/CodeBuild · Cloudflare

Sole platform engineer for a crypto market-data platform: multi-terabyte Aurora, continuous
ingestion from exchanges and on-chain sources.

- **Piloted autonomous AI-assisted development** on the platform: built a devcontainer-based
  environment to let agents run with reduced blast radius, supported by tight feedback loops
  and thorough agent-focused documentation. Now used for day-to-day delivery.
- Built **Datadog observability from scratch** (APM, logs, metrics, monitors), replacing a
  fragmented setup; extended coverage to AWS Nitro Enclave workloads via Vector.
- Responded to production incidents: diagnosed and resolved **recurring OOMs and a site-wide
  outage**, tracing both to specific upstream data conditions.
- Introduced **Pulumi IaC** with AWS OIDC; imported existing hand-managed resources into
  managed stacks.
- Identified SQL patterns driving **~33% of database cost**; cut worst-case query time from
  **4h to 20s** with composite indexes and aggregate rewrites.
- Cut **AWS spend by 10%**: EC2 right-sizing, CloudWatch rationalisation, ECR lifecycle
  policies, Aurora migrated to Graviton, and recurring load spikes flattened by redistributing
  cron schedules.
- Reduced web tail latency **75% (20s → 5s)** and halved Sidekiq infrastructure through
  instance-type and queue tuning.

### Wellfound — Senior Software Engineer, Cloud Infrastructure

*Jan 2020 – Oct 2025 · Remote (Germany / Spain)*

Kubernetes (EKS) · Terraform · AWS · ArgoCD · Helm · Datadog · Cloudflare · Buildkite ·
Docker · Ruby on Rails · GraphQL · Elasticsearch · Redis · MySQL · Postgres · Python / Dagster

One of **two infrastructure engineers supporting 20+ product engineers**, owning reliability,
scalability and delivery pipelines for a platform serving 3M+ monthly visitors.

- Operated and upgraded **3 production EKS clusters**, plus Ruby, Rails and Elasticsearch,
  **without downtime**; managed cluster dependencies and release lifecycle with Helm.
- Ran **GitOps delivery with ArgoCD**; set up **canary deployments** for the main application.
- Built **near-instant rollback** into the data team's Dagster pipeline (Python): clone the
  ~1TB Elasticsearch index before each multi-hour run, repoint the alias on failure. Recovery
  went from a **4h+ snapshot restore to seconds**, letting the data team ship changes without
  risking a corrupted production index.
- Shared a **week-long on-call rotation** with the other infrastructure engineer; authored
  **runbooks and postmortems**. Drove incident frequency **from monthly to a few per year**
  through caching strategies, DB read/write splitting and resource tuning.
- Managed all infrastructure as code in **Terraform**: split a monolithic workspace into
  **smaller, well-scoped workspaces** with clear ownership and blast-radius boundaries; cut
  infrastructure costs by **~10%**.
- Migrated observability from **ELK to Datadog**, designing dashboards and alerting; ran
  internal workshops to upskill the engineering team.
- Optimised CI/CD pipelines, cutting build times by **80% and 50%** on the two most
  business-critical applications.
- Reduced key endpoint latencies **from seconds to sub-100ms** (GraphQL overfetching across
  datasources, N+1 queries, missing indexes).
- Drove **SOC 2 compliance**, getting the company attested in a couple of months vs. the
  typical 6–12 month timeline.

### New Work SE — Senior Software Engineer, API Platform

*Dec 2016 – Dec 2019 · Germany*

Scala · GraphQL · Ruby on Rails · Kubernetes · Docker · Prometheus · RabbitMQ

**First engineer** on the platform team that gave product teams the infrastructure to expose
their APIs. The team grew from 2 to ~8 engineers during my time. Owned the existing REST
gateway while designing and building its GraphQL successor.

- Designed and built a new **GraphQL API gateway in Scala**, learning both Scala and GraphQL
  on the job and prototyping in Elixir. It became core infrastructure adopted by multiple
  teams (**100+ engineers**).
- **Ran the gateway as an internal product:** features shipped because product teams needed
  them, backed by extensive documentation and **onsite workshops delivered across offices in
  Germany, Spain and Portugal**.
- **Presented the platform work publicly** at a meetup in Germany and a conference in Portugal.
- Created **internal developer content** (written guides plus screencasts and feature demos on
  the internal blog) to scale adoption past what in-person support could reach.
- Onboarded product teams progressively rather than all at once, keeping platform-team
  bandwidth realistic and the tooling solid at every step.
- Maintained the legacy **Rails REST gateway serving 10k+ req/s** throughout the transition so
  teams could migrate at their own pace, and **migrated it to Kubernetes with zero downtime**.

### WyeWorks — Software Engineer

*May 2015 – Nov 2016 · Uruguay*

- Built Rails-based web applications for startups, focused on clean architecture and
  performance.

### Earlier

**2008–2015:** Software Engineer at Scanntech, Holla@Me, Crambo and Universidad de Alcalá
(mobile & web applications).

## Technical skills

- **Cloud & IaC:** AWS (EKS, EC2, S3, ECR, IAM, ALB, RDS/Aurora, SSM, VPC) · Terraform ·
  Pulumi · Cloudflare
- **Containers & Delivery:** Kubernetes / EKS · Helm · ArgoCD · GitOps · canary & progressive
  delivery · Docker
- **CI/CD:** Buildkite · GitHub Actions · CodePipeline/CodeBuild
- **Observability & Reliability:** Datadog · Prometheus · Rollbar · on-call & incident response
  · runbooks & postmortems · SOC 2 (Vanta)
- **Data:** Postgres / Aurora · MySQL · Elasticsearch · Redis
- **Edge & Security:** Cloudflare · DataDome · AWS OIDC · Secrets Manager / 1Password / dotenvx
- **Languages:** Ruby · Python · TypeScript · Scala · Java · Bash

## Education

**Universidad de Alcalá** — Bachelor of Engineering, Computer Science · 2005–2012 · Spain
