---
title: Overview
description: Case Study — One Verse Daily, a Class 1.1 content application built on VueJS with a first-class NestJS backend. An early adoption of the full five-stratum STRATA sequence, predating the protocol's formal specification.
status: early-access
navigation:
  order: 0
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-30"
---

:status-badge{status="early-access"}

One Verse Daily (oneversedaily.com) is a liturgically-aware Scripture formation platform: one carefully curated Bible verse each day, presented with scholarly commentary, cultural context, and a liturgical interface that teaches the rhythms of the Church year through color. Its governing principle is *One verse. One day. Infinite depth.*, and its stated measure of success is formation rather than engagement — "saints, not users." Beneath a deliberately thin Vue interface sits real depth: a perpetual liturgical-calendar engine computed rather than looked up, a multi-version Bible model with genuine canon handling, a cookie-based authentication stack with two-factor sign-in, and guest-first encrypted drafts that migrate into an account on registration.

It was built before STRATA was specified — a side project delivered in evenings and weekends — yet it went from an empty repository to ten feature releases in roughly five to six weeks without the usual side-project entropy. The reason was a disciplined, repeatable, AI-assisted workflow: the primitive ancestor of the protocol.

## Classification: Class 1.1 — Greenfield MVP

One Verse Daily is classified **Class 1, Category 1.1 — Greenfield MVP** — a new product entering an unvalidated market. There was no existing system whose continuity had to be preserved and no proven demand to expand on; the project was testing whether a beauty-first, formation-first devotional could find its audience. That is exactly the posture Class 1.1 governs: move fast, keep scope tight, and let a clear product vision carry the governance a mature platform would formalize.

## Technology Stack

| Concern | Choice |
|---|---|
| Frontend | VueJS single-page app — Vue 3, TypeScript, Vite, Tailwind, with Vitest and Playwright |
| Backend | NestJS — a shared "Platform-First" core providing multi-tenant auth, entitlements, and AI services |
| Architecture | Clean-architecture boundaries: a framework-agnostic domain core, feature-scoped modules, and design-system primitives |

## STRATA Principles Observed

Nothing here was called a stratum, yet all five were present:

- **Classification** — the project's risk posture (greenfield, speed-primary, change-tolerant) set in its founding manifesto and vision.
- **Derivation** — documents derived in dependency order: manifesto → vision → functional requirements → per-feature analysis and plan.
- **Authority chain** — a single AI-readable constitution distilled from the manifesto-to-standards chain, inherited by every AI session.
- **Execution loop** — a four-stage, phase-gated lifecycle with a human review gate before any feature code was written, and a corrections-driven build loop.
- **Artifact trail** — per-feature manifests, running commit logs, preserved prompts with cost annotations, and deviation notes.

::tip
The clarity of the product vision — "saints, not users" — was the load-bearing document every later decision had to cite. That is precisely what Stratum 2 derivation formalizes: a Vision and Business Case at the head of the chain, with everything downstream required to align with it. One Verse Daily had the substance informally; STRATA makes it official.
::

## What STRATA Would Have Formalized

The founding manifesto would become the official Vision and Business Case at the top of the authority chain. The "denominational silos" problem the product was built to solve would be a classified risk in a Business Requirements Document. The implicit greenfield posture would be a written Classification Record. None of this would have changed the product — it would have made the governance reusable, so the next build started from a template instead of from instinct. See [Derivation](/documentation/the-strata/stratum-2-derivation) for the stratum this project ran on conviction alone.

::u-page-c-t-a
---
title: Go deeper than the summary
description: The full five-stratum walkthrough of One Verse Daily — every layer, every decision — plus the raw development journal behind the build.
variant: soft
links:
  - label: Read the full case study
    to: /case-studies/one-verse-daily/full
    icon: i-ph-lock
    color: primary
  - label: Open the development journal
    to: /case-studies/one-verse-daily/journal
    icon: i-ph-notebook
    color: neutral
---
::
