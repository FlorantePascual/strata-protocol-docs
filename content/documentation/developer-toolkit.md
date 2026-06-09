---
title: Developer Toolkit
description: A public preview of the STRATA Protocol Developer Guide — platform architecture, governed delivery workflow, and the practitioner resources available to members building STRATA-governed systems.
status: preview
navigation:
  order: 8
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-30"
---

:status-badge{status="preview"}

The Developer Guide is the implementation layer of the STRATA Protocol — where the framework's governance principles become concrete templates, workflows, and reference architecture for teams building AI-assisted software. This page previews what the Guide contains, structured around three questions: how the reference platform is built, how features are governed, and how code changes flow through the layers.

## Platform Architecture

The STRATA Protocol reference platform — StrataProtocol.org itself — is a Nuxt 4 / Docus SSR application. Four distinct layers handle content, presentation, utilities, and server-side request processing:

::strata-diagram{name="devguide_architecture" alt="Reference architecture: four horizontal layers — Content layer (Markdown and MDC pages), App layer (Vue components, composables, app middleware), Shared layer (pure TypeScript utilities), Server layer (Nitro API routes and Content Guard RBAC). A SQLite store and sealed HttpOnly session cookie are connected to the Server layer."}
::

Content lives at the top as Markdown and MDC pages — the source material. The App layer renders content through Vue components and composables, with app middleware enforcing client-side route guards. The Shared layer provides pure TypeScript utilities consumed by both App and Server. The Server layer runs Nitro API routes and the content guard — a server-side RBAC middleware that validates the user's session role before serving protected content.

External state is kept minimal: a SQLite database for member records and a sealed HttpOnly session cookie for authentication. No third-party auth services. No session data exposed to the client.

## Governed Delivery Workflow

Every feature on this platform was built using the STRATA governed delivery workflow — the same workflow the Developer Guide documents for practitioners to apply on their own projects. Five stages, each producing a durable governance artifact:

::strata-diagram{name="devguide_workflow" alt="Governed delivery workflow: five sequential stages — Classify (Classification Record), Derive (Vision, BRD, PRD, Standards), Plan (Analysis doc, Phase plan), Execute (Phase notes, Manifest), Validate (Gate pass, Human approval). A feedback arrow runs from Validate back to Plan, showing the governed iteration cycle."}
::

The feedback arrow from Validate to Plan is the key structural feature: this platform does not gate on perfect first-pass delivery. It gates on human approval at each phase boundary. Failures loop back to Plan, the analyst re-examines scope, the AI copilot re-implements, and the gate re-runs. This is the governed delivery loop in practice — and it is self-referential: the loop you are looking at is the loop that built the page you are reading.

## Implementation Flow

When a feature is implemented, code changes move through the platform layers in a specific, testable sequence:

::strata-diagram{name="devguide_implementation_flow" alt="Implementation flow: six nodes top to bottom — MDC Content Page, Vue Content Component, App Composable, Shared Utility, Nitro API Route, Server Middleware (RBAC). Downward arrows connect each layer. A right-margin dashed test lane shows co-located spec files connected to the Shared Utility and Server Middleware layers."}
::

Shallow changes touch only the top layers — a new MDC page or a content component. Deeper changes propagate down through composables and shared utilities to the server. The test lane on the right is structural: co-located `*.spec.ts` files live at the Shared and Server layers — the two layers where testable logic lives. Content and App layer changes are validated through type-check and build alone.

## Available to Members

A member account unlocks the full Developer Guide — practitioner resources for applying this architecture on real projects:

::card-group
  ::card{title="Deep-Dive Documentation" icon="i-ph-book-open-text" to="/documentation/deep-dives"}
  Five procedural guides — one per stratum — covering how to apply STRATA from project classification through artifact preservation. Available now.
  ::
  ::card{title="Implementation Templates" icon="i-ph-files" to="/developer-guide"}
  Governance document templates derived directly from `docs/strata/`: Classification Records, BRDs, PRDs, Phase Plans, and Implementation Manifests — with usage notes and contextual examples. The template library is expanding incrementally (REQ-006).
  ::
  ::card{title="AI Session Guides" icon="i-ph-robot" to="/ai-playbook/guides"}
  Structured prompt sets for working with GitHub Copilot, Cursor, and Claude Code at each stratum — designed to keep AI tools within the authority chain.
  ::
::

## Expanding

The Developer Guide template library is being built incrementally under REQ-006. Current development priorities:

- Nuxt / Docus documentation platform templates — the reference stack shown above
- NestJS / Vue application templates with STRATA governance wired in
- Guided worked examples: one complete project from classification through artifact trail
- Stack-specific authority chain mandates for GitHub Copilot, Cursor, and Claude Code

See the [Platform Roadmap](/changelog/roadmap) for the current build state of each template set.

::u-page-c-t-a
---
title: Access the full Developer Guide
description: Implementation templates, reference architecture, and guided workflows for building STRATA-governed systems — available to members today, with more added each release.
variant: soft
links:
  - label: Access with a member account
    to: /documentation/account
    icon: i-ph-lock
    color: neutral
---
::
