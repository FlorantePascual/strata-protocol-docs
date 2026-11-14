---
title: Introduction
description: How the STRATA implementation templates work — three-step process, stack selection, and how each template maps to the five strata.
navigation:
  order: 1
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-06-02"
---

The STRATA implementation templates give you the exact documents that practitioners use to govern real features from idea to shipped code. Each template covers the complete decision surface for a feature: what it affects structurally, how data flows, what the security model requires, how tests should be organized, and how work phases should be sequenced.

Templates work because they force systematic thinking before code is written. A practitioner who completes the structural analysis before opening a code editor will have already resolved the questions that typically surface as bugs, missed requirements, or integration failures mid-sprint.

## The Three-Step Process

STRATA governs work one [Feature Release](/documentation/reference#feature-release) at a time, and every release follows the same three-step sequence, regardless of stack:

**Step 1 — Analysis.** Before a line of code is written, you complete the analysis template. This covers the structural footprint — which files, routes, API endpoints, database tables, and components are affected — along with the data model, API contract, security considerations, and a rough test specification. The analysis is not a design document; it is an impact inventory. Its job is to surface everything the release touches before the cost of touching it becomes real.

**Step 2 — Implementation Plan.** The analysis feeds directly into the implementation plan. This document converts the structural inventory into a phased delivery sequence: what is built in which order, what the acceptance criteria are for each phase, and what validation steps confirm each phase is complete. The plan is the agreement between the developer and the governance record.

**Step 3 — Execution.** The implementation plan drives execution, one phase at a time. Each phase gate is a checkpoint where the criteria from the plan are verified. Deviations are noted explicitly. The execution loop produces the running commit log and phase manifests that form the artifact trail.

The analysis and the plan are written when the release's execution session opens, not months ahead alongside the requirement, so they are drawn up against the code as it stands and everything earlier releases learned. The Developer Guide templates cover Step 1 (analysis) and Step 2 (implementation plan) for each supported stack. Step 3 execution artifacts are the developer's own output — the templates establish the standard those artifacts must meet.

## Choosing Your Template

Three template sets are available, each specific to a technology stack:

**Nuxt / Docus Full-Stack** — for SSR documentation portals and full-stack Nuxt applications. Use this template when your project uses Nuxt 4 (often via Docus), Nuxt Content, server-side rendering, and Nitro server middleware. StrataProtocol.org itself is the canonical reference implementation.

**NestJS Backend** — for API-first, module-based backend services. Use this template when your project is a NestJS application with controllers, services, guards, and an ORM (TypeORM or Prisma) connecting to a relational database. Appropriate for microservice components, enterprise APIs, and standalone backends consumed by a separate frontend.

**Vue Frontend** — for Vue 3 SPAs consuming a REST or GraphQL API. Use this template when your project is a Composition API Vue application using Pinia for state management and Axios or fetch for data access — a separate frontend codebase with no server-side rendering.

::note
If your project combines a NestJS backend and a Vue frontend, run both templates in sequence — one for each layer. The analysis documents are kept separate because the structural footprint, data model, and security concerns are distinct per layer, and conflating them obscures responsibility boundaries.
::

## How Templates Map to the STRATA Strata

Templates live inside **Stratum 4 — Execution Loop**, the stratum that governs the delivery of each Feature Release. But they reference all five strata:

- **Stratum 1 (Classification)** — the project's classification record sets how deep the derivation behind each release goes and declares the constitution editions the work runs under. The analysis inherits that record; it does not reclassify.
- **Stratum 2 (Derivation)** — the template header traces the release to its record in the product requirements and to the requirements it satisfies, confirming the work is derived, not improvised.
- **Stratum 3 (Authority Chain)** — the session runs under the operative constitution, issued as one edition per AI agent, and the project's standards. The analysis calibrates against the quality gate and stops for a ruling when a requirement and a standard conflict.
- **Stratum 4 (Execution Loop)** — the implementation phases section defines the execution sequence, acceptance criteria, and phase gates.
- **Stratum 5 (Artifact Trail)** — the release's session folder mirrors the path of its requirement record, and the phase notes, manifests and running commit log it accumulates become the release's trail.

Filling in a STRATA template is not administrative overhead — each section corresponds to a governance obligation that exists whether or not it is documented. The template makes the obligation explicit and verifiable.

## Adapting Templates for Different Project Types

Templates are stack-specific, not project-size-specific. A small feature and a large feature use the same template structure — the difference is in the depth of each section, not the presence or absence of sections. A section that does not apply to a given feature should be marked "Not applicable" with a brief rationale, not deleted. Preserved structure tells future reviewers the section was considered and intentionally skipped, not overlooked.

When working on a project that does not exactly match one of the three supported stacks, choose the closest match and record the deviations in the analysis. The governance value comes from the structure, not from perfect stack alignment.

::u-page-c-t-a
---
title: Ready to use STRATA templates on your next feature?
description: Professional members get full access to the implementation templates, step-by-step quickstart guidance, and the complete reference catalog.
variant: soft
links:
  - label: Create a free account
    to: /login
    icon: i-ph-user-plus
    color: neutral
  - label: Go Professional
    to: /login
    icon: i-ph-rocket-launch
    color: primary
---
::
