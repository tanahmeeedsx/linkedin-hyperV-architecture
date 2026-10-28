<!--
  LinkedIn Hyper V Architecture
  DEV-774 | Architecture Exploration & Improvement Planning
-->

<div align="center">

  <img src="https://cdn.simpleicons.org/linkedin/0A66C2" alt="LinkedIn logo" width="76" height="76" />

  <h1>LinkedIn Hyper V Architecture</h1>

  <p><strong>Architecture Exploration · DevOps · Reliability · Type Safety</strong></p>

  <p>
    <img src="https://img.shields.io/badge/Project-Architecture%20Research-0A66C2?style=for-the-badge" alt="Architecture research" />
    <img src="https://img.shields.io/badge/Status-In%20Progress-orange?style=for-the-badge" alt="Status: In Progress" />
    <img src="https://img.shields.io/badge/Initiative-DEV--774-555555?style=for-the-badge" alt="Initiative DEV-774" />
  </p>

  <p>A structured workspace for understanding the existing LinkedIn Hyper V codebase and planning improvements to architecture, delivery pipelines, observability, and reliability.</p>

</div>

---

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Architecture at a Glance](#architecture-at-a-glance)
- [Existing and Proposed Architecture](#existing-and-proposed-architecture)
- [Technology Stack](#technology-stack)
- [Areas of Exploration](#areas-of-exploration)
- [Implementation Roadmap](#implementation-roadmap)
- [Development Workflow](#development-workflow)
- [Validation Checklist](#validation-checklist)
- [Repository Structure](#repository-structure)
- [Progress and Next Steps](#progress-and-next-steps)
- [References](#references)

## Project Overview

**Initiative:** DEV-774 — Improvement of LinkedIn Hyper V  
**Upstream project:** [Acumen-org/Linkedin-Hyper-V](https://github.com/Acumen-org/Linkedin-Hyper-V)  
**Purpose of this repository:** Architecture research, technical notes, diagrams, and improvement planning.

The upstream project uses a Next.js dashboard alongside a Node.js background worker for LinkedIn-related data synchronization, messaging, and automation. The DEV-774 initiative aims to improve consistency between services, deployment workflows, observability, testing, and data-processing reliability.

This is a **companion research workspace**, not a replacement for the upstream application. Proposed designs and roadmap items remain planning notes until verified against the codebase and implemented in the appropriate project repository.

## Objectives

- Understand the dashboard, worker, API, and data-flow boundaries.
- Evaluate migration of worker modules from JavaScript to TypeScript.
- Plan shared contracts for API payloads, worker messages, and data models.
- Review Docker and CI/CD workflows for repeatable builds and deployments.
- Improve service visibility through structured logs and health checks.
- Plan reliable event processing, retry behavior, and message deduplication.
- Define practical testing and validation steps before implementation.

## Architecture at a Glance

The diagram below is a **high-level conceptual view** based on the epic description. Confirm actual connections, queues, databases, and deployment targets by inspecting the upstream codebase.

~~~mermaid
flowchart LR
    U[User / Operator] --> UI[Next.js Dashboard]
    UI --> API[Application API / Proxy]
    API --> W[Node.js Background Worker]
    W --> EXT[External LinkedIn Interfaces]
    W --> DB[(Prisma-backed Data Store)]
    W --> BUS[Events / Webhooks]
    BUS --> CON[Persistence Consumer / External Consumers]

    UI -. planned shared contracts .-> TYPES[Shared TypeScript Types]
    W -. planned shared contracts .-> TYPES
    CI[GitHub Actions CI] -. validate .-> UI
    CI -. validate .-> W
    OBS[Structured Logs / Health Checks] -. observe .-> API
    OBS -. observe .-> W
~~~

### Core components

| Component | Responsibility to verify |
|---|---|
| Next.js dashboard | User interface and application-side workflows |
| API / proxy layer | Request handling and communication with background processing |
| Background worker | Asynchronous jobs and integration logic |
| Prisma and data store | Persistence and database access |
| Event bus and consumers | Event delivery, persistence, and external notifications |
| GitHub Actions | Automated linting, type checking, tests, and build validation |
| Observability | Structured logs, health status, and operational troubleshooting |

## Existing and Proposed Architecture

| Area | Current understanding | Improvement direction |
|---|---|---|
| Language consistency | Dashboard and worker have different levels of TypeScript adoption | Move worker modules toward TypeScript and strict type checks |
| Shared contracts | Data and message contracts may be duplicated across services | Establish a reusable shared types/contracts layer |
| Activity policies | Rate limits and behavior-related logic exist across separate modules | Review policy ownership and coordinate actions through explicit, testable rules |
| Proxy resilience | Proxy health and rotation logic need review | Add observable health checks and well-defined failure handling |
| Deployment | Shell scripts and multiple Nomad definitions are described in the epic | Consolidate deployment configuration and document environment variables |
| CI/CD | Validation workflow needs standardization | Add repeatable lint, type-check, test, and Docker build stages |
| Logging | Logs need consistent structure across components | Use structured JSON logging and correlation identifiers where appropriate |
| Event reliability | Webhook delivery and deduplication need stronger resilience | Add bounded retries, backoff, idempotency, and a dead-letter strategy |

These are improvement targets from the epic, not claims that the changes have already been completed.

## Technology Stack

The following tools are relevant to the investigation. Their actual use and versions should be confirmed from repository files such as package manifests, lockfiles, Dockerfiles, and deployment manifests.

<div align="center">
  <img src="https://cdn.simpleicons.org/nextdotjs/000000" alt="Next.js" height="42" />
  &nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/nodedotjs/339933" alt="Node.js" height="42" />
  &nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/typescript/3178C6" alt="TypeScript" height="42" />
  &nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/docker/2496ED" alt="Docker" height="42" />
  &nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/githubactions/2088FF" alt="GitHub Actions" height="42" />
  &nbsp;&nbsp;
  <img src="https://cdn.simpleicons.org/nomad/00CA8E" alt="Nomad" height="42" />
</div>

| Technology / concept | Why it matters here |
|---|---|
| Next.js | Dashboard architecture and frontend workflows |
| Node.js | Background worker runtime and service integrations |
| TypeScript | Compile-time checks and safer refactoring |
| Prisma | Database access and data-model consistency |
| Docker | Repeatable build and runtime environments |
| GitHub Actions | Automated quality checks and CI pipeline |
| Nomad | Deployment job configuration described in the epic |
| Pino or equivalent | Structured JSON application logging |
| Unit and integration testing | Verify policies, worker behavior, and data integrity |
| Retry and dead-letter patterns | Recover from temporary delivery failures and track persistent failures |

## Areas of Exploration

### 1. Codebase and TypeScript

- Map main directories, application entry points, and worker modules.
- Inspect package manifests, TypeScript configuration, lint rules, and lockfiles.
- Identify JavaScript worker files and their dependencies.
- Plan shared interfaces for API responses, IPC/Redis messages, and data models.
- Run existing checks before changes to establish a baseline.

### 2. CI/CD and Deployment

- Inspect current workflows under .github/workflows/.
- Review Dockerfiles, Compose files, deployment scripts, and Nomad job files.
- Identify duplicated configuration and inconsistent environment variables.
- Plan a CI workflow that runs linting, type checks, tests, and Docker build validation.
- Keep credentials in approved secret stores; never commit tokens or production secrets.

### 3. Observability and Operations

- Identify worker and API health-check opportunities.
- Review existing logs and define consistent JSON fields, severity levels, and request/job identifiers.
- Determine which failures should trigger alerts and what operational context is needed.
- Avoid logging credentials, session cookies, tokens, or other sensitive data.

### 4. Testing and Data Reliability

- Locate current unit and integration tests.
- Review message deduplication and idempotency behavior.
- Define tests for rate-limit boundaries and coordinated job execution.
- Plan bounded exponential backoff and dead-letter handling for repeated webhook failures.
- Verify that retries cannot unintentionally duplicate side effects.

### 5. Activity Policies and Account Safety

- Understand how rate limits, session state, proxy health, and job scheduling are represented.
- Prefer explicit, centralized, testable policy decisions over scattered checks.
- Design for safe failure, observability, and respect for LinkedIn's terms and access limits.
- Do not treat anti-detection or ban avoidance as a guarantee; no architecture can guarantee account longevity.

## Implementation Roadmap

The order below is a proposed workflow. Priorities should be aligned with the assigned task and the upstream team's review.

| Phase | Work | Expected output |
|---|---|---|
| 1. Discover | Inspect directories, dependencies, scripts, and existing checks | Architecture notes and baseline findings |
| 2. Map | Document service boundaries and data/event flows | Verified architecture diagram |
| 3. Type safety | Identify migration scope and shared contract boundaries | Incremental TypeScript migration plan |
| 4. CI/CD | Review workflows and deployment duplication | CI checklist and deployment standardization plan |
| 5. Observability | Review logs, health checks, and failure signals | Logging and monitoring design |
| 6. Reliability | Review retries, deduplication, and failure recovery | Test scenarios and resilience plan |
| 7. Validate | Run agreed checks for each change | Test evidence and documented results |

## Development Workflow

### 1. Clone the upstream project for inspection

~~~bash
git clone https://github.com/Acumen-org/Linkedin-Hyper-V.git
cd Linkedin-Hyper-V
~~~

Use the team's approved access method if the repository is private. Do not place credentials in clone URLs.

### 2. Inspect before changing code

~~~bash
git status
git branch --show-current
find . -maxdepth 2 -type f | sort | head -200
~~~

Review relevant configuration files and existing documentation before deciding what to modify. Run only the checks supported by the project scripts and local environment.

### 3. Work in small, reviewable changes

- Confirm the assigned scope with the project mentor.
- Create a focused branch in the upstream repository when authorized.
- Make one logical change at a time.
- Run relevant lint, type-check, test, and build commands.
- Document findings and evidence without including secrets.
- Open a pull request for team review rather than bypassing the project's workflow.

### 4. Keep research separate from production changes

This repository stores architecture notes and planning material. Implementation changes should be made in the upstream repository or the team's explicitly approved fork.

## Validation Checklist

- [ ] Existing repository structure and scripts reviewed
- [ ] Current checks and baseline failures documented
- [ ] Architecture diagram checked against the actual code
- [ ] TypeScript migration scope identified
- [ ] Shared contract boundaries documented
- [ ] CI workflow requirements mapped to available scripts
- [ ] Docker and deployment configuration reviewed
- [ ] Logging and health-check requirements identified
- [ ] Retry, idempotency, and deduplication scenarios defined
- [ ] Tests run and results recorded for implemented changes
- [ ] Changes reviewed and approved by the team

## Repository Structure

The companion repository can grow with the investigation:

~~~text
linkedin-hyperV-architecture/
├── README.md
├── docs/
│   ├── architecture/
│   │   └── system-overview.md
│   ├── findings/
│   │   └── initial-review.md
│   ├── ci-cd/
│   │   └── pipeline-review.md
│   ├── reliability/
│   │   └── testing-and-retry-strategy.md
│   └── images/
│       └── README.md
└── notes/
    └── glossary.md
~~~

This is a suggested structure; create supporting files as findings are verified rather than adding empty files just to match the tree.

## Progress and Next Steps

**Current status:** Architecture exploration and technical research in progress.

Initial focus:

1. Understand the upstream codebase and confirm repository access.
2. Verify architecture and technology choices from actual files.
3. Identify the first assigned improvement and agree on its scope.
4. Record the baseline, implement incrementally, and test each change.
5. Keep the README updated with verified findings and implementation status.

Completed implementation should only be recorded after code changes and their validation have actually been completed.

## References

- [Upstream LinkedIn Hyper V repository](https://github.com/Acumen-org/Linkedin-Hyper-V)
- [TypeScript documentation](https://www.typescriptlang.org/docs/)
- [Docker documentation](https://docs.docker.com/)
- [GitHub Actions documentation](https://docs.github.com/en/actions)
- [HashiCorp Nomad documentation](https://developer.hashicorp.com/nomad/docs)
- [Prisma documentation](https://www.prisma.io/docs)

---

<div align="center">
  <p><strong>DEV-774 · LinkedIn Hyper V Architecture Exploration</strong></p>
  <p><sub>Research, document, validate, and improve incrementally.</sub></p>
</div>
