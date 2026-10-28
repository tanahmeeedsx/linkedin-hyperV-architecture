<div align="center">

# LinkedIn Hyper V Architecture

**DEV-774 · Architecture Research & Improvement**

<img src="https://skillicons.dev/icons?i=nextjs,nodejs,ts,docker,githubactions,prisma" alt="Next.js, Node.js, TypeScript, Docker, GitHub Actions, Prisma" />

<br/>

![Status](https://img.shields.io/badge/Status-Exploration%20in%20Progress-2563EB?style=flat-square)
![Focus](https://img.shields.io/badge/Focus-DevOps%20%7C%20Reliability-0F766E?style=flat-square)

*Understanding the system. Planning improvements. Validating every change.*

</div>

---

## Overview

A companion research workspace for **DEV-774 — Improvement of LinkedIn Hyper V**. The upstream project combines a **Next.js dashboard** with a **Node.js background worker** for LinkedIn-related workflows.

This repository documents architecture findings, tools, and implementation plans. Actual changes belong in the [upstream repository](https://github.com/Acumen-org/Linkedin-Hyper-V) or an approved team fork.

## Technology Stack

| Technology | Purpose |
|---|---|
| Next.js + Node.js | Dashboard and background worker |
| TypeScript | Type safety and consistent contracts |
| Prisma | Data access and model consistency |
| Docker | Reproducible builds and runtime |
| GitHub Actions | Automated CI checks |
| Nomad | Deployment configuration |
| Pino / structured logging | Service visibility and troubleshooting |

*The epic references these technologies; exact usage and versions will be verified from the source code.*

## Architecture Overview

~~~mermaid
flowchart LR
    UI[Next.js Dashboard] --> API[API / Proxy]
    API --> W[Node.js Worker]
    W --> DB[(Prisma / Database)]
    W --> E[Events & Webhooks]
    E --> C[Consumers]
    T[Shared TypeScript Contracts] -. planned .-> UI
    T -. planned .-> W
    CI[GitHub Actions] -. validates .-> UI
    CI -. validates .-> W
~~~

*Conceptual view based on the epic; connections must be confirmed against the codebase.*

## Improvement Focus

- **Type safety:** Migrate worker modules to TypeScript and define shared contracts.
- **CI/CD:** Standardize linting, type checks, tests, and Docker build validation.
- **Deployment:** Review scripts, Nomad definitions, and environment configuration.
- **Observability:** Add consistent structured logs and health checks.
- **Reliability:** Test retries, idempotency, webhook delivery, and message deduplication.
- **Operational safety:** Make rate-limit and job-execution policies explicit, testable, and respectful of platform rules.

## Workflow

1. **Explore** — inspect the repository, scripts, dependencies, and current checks.
2. **Map** — verify service boundaries and data flow.
3. **Prioritize** — agree on a focused improvement with the project mentor.
4. **Implement** — make small, reviewable changes in the approved codebase.
5. **Validate** — run relevant checks and document results.

## Current Status

**In progress:** Architecture exploration and technical research.

**Next:** Verify the actual codebase, record baseline findings, and begin the first agreed implementation task. Items above are planned improvements, not claims of completed implementation.

## References

- [Upstream project](https://github.com/Acumen-org/Linkedin-Hyper-V)
- [TypeScript](https://www.typescriptlang.org/docs/) · [Docker](https://docs.docker.com/) · [GitHub Actions](https://docs.github.com/en/actions)
- [Prisma](https://www.prisma.io/docs) · [Nomad](https://developer.hashicorp.com/nomad/docs)

---

<div align="center">
<sub>DEV-774 · Explore clearly. Improve incrementally. Validate thoroughly.</sub>
</div>
