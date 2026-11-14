---
title: Overview
description: Case Study — building the STRATA Protocol documentation platform using the STRATA Protocol itself. A self-referential Class 1.3 Platformization project on Nuxt Content and Docus, with a complete five-stratum artifact trail.
status: stable
navigation:
  order: 0
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-30"
---

:status-badge{status="stable"}

StrataProtocol.org is the official platform for the STRATA Protocol — the five-stratum governance framework for AI-assisted software delivery. It documents the framework in public, gives members implementation templates and prompts, and hosts the case studies you are reading now. It is also the framework's first and most complete application: every page on this site was produced under the same classification, derivation, authority chain, execution loop, and artifact trail that the site teaches.

That is the point of this case study. Most documentation describes a method and asks you to trust that it works. StrataProtocol.org was built by running the method on itself — and the receipts are not a marketing claim, they are inspectable. The governed artifacts that produced this platform are public, open for inspection in the project's documentation repository.

## A Platform That Is Both the Subject and the Proof

StrataProtocol.org occupies two roles at once. It is the **subject** — the thing that teaches the STRATA Protocol — and it is the **proof** — a real, shipped project governed by that protocol end to end. The two are not separate exhibits bolted together; they are the same artifacts seen from two angles. The Classification Record this case study points to is the actual record that fixed the project's class before a line of specification was written. The Vision, the Business Requirements, and the authority chain are the originals that governed the build, not illustrative samples written after the fact.

This is why the case study sits at the top of the list: it is the strongest credibility signal STRATA can offer. A practitioner reading about the Artifact Trail concept in the documentation can open this page and immediately inspect a real one — the very trail that produced the page in front of them.

::tip
The medium is the message. This platform's own Stratum 5 — its artifact trail — is published as public content. You are not reading a description of governed delivery; you are reading its output, with the governance record one click away. Proof over assertion is the whole argument, and here the proof is the product.
::

## Classification: Class 1.3 — Platformization

StrataProtocol.org is classified **Class 1, Category 1.3 — Platformization**. Class 1 is correct because the business is still discovering and validating its market form — there are no paying members, no contractual obligations, and no existing system whose continuity must be preserved.

The category is Platformization, not Greenfield MVP (1.1), because the asset being productized is already validated. The STRATA methodology was proven through direct engineering practice before this platform existed. The initiative does not test an unproven hypothesis; it *converts* a validated internal capability into an accessible, scalable, externally-facing platform — a methodology becomes a documentation platform, internal templates become a member toolkit, and a personal engineering practice becomes a framework available to practitioners worldwide.

That classification set the engineering priorities for everything downstream: the content architecture had to generalize across project types (abstraction), the templates had to be genuinely actionable rather than illustrative (developer experience), and the documentation quality had to be defensible against the very framework it describes (reusability and the self-referential proof). A Greenfield MVP classification would have permitted thinner content, rough navigation, and placeholder-heavy docs — acceptable trade-offs for an unvalidated experiment, but contradictions for a platform whose purpose is to demonstrate best-in-class practice.

## The Artifact Trail

This platform was built under STRATA Protocol v1.0, which numbered requirements `REQ-NNN`, and its records keep those identifiers; FR-001 is its first [Feature Release](/documentation/reference#feature-release) under STRATA Protocol v1.2.

The governance artifacts that produced this platform are public in the [strata-protocol-docs repository](https://github.com/FlorantePascual/strata-protocol-docs){target="_blank" rel="noopener"} under `docs/strata/` — the platform's application source is private, but the records that governed the build are open. Each card below opens its artifact in the repository, in a new tab:

::card-group
  ::card{title="Stratum 1 — Classification Record" icon="i-ph-stack" to="https://github.com/FlorantePascual/strata-protocol-docs/blob/main/docs/strata/1.classification/classification-record.md" target="_blank"}
  The record that fixed this project as Class 1.3 — Platformization before any specification was written.
  ::
  ::card{title="Stratum 2 — Vision & Business Case" icon="i-ph-compass" to="https://github.com/FlorantePascual/strata-protocol-docs/blob/main/docs/strata/2.derivation/01-vision/vision-business-case.md" target="_blank"}
  Layer 1 (Why): the problem, the opportunity, and the business objectives that justify the platform.
  ::
  ::card{title="Stratum 2 — Business Requirements" icon="i-ph-list-checks" to="https://github.com/FlorantePascual/strata-protocol-docs/blob/main/docs/strata/2.derivation/03-brd/business-requirements.md" target="_blank"}
  Layer 2 (What): the business requirements derived from the vision.
  ::
  ::card{title="Stratum 2 — Product Requirements" icon="i-ph-files" to="https://github.com/FlorantePascual/strata-protocol-docs/tree/main/docs/strata/2.derivation/04-prd" target="_blank"}
  Layer 3 (How): the numbered product requirements that drive each governed execution sprint.
  ::
  ::card{title="Stratum 3 — Authority Chain" icon="i-ph-shield-check" to="https://github.com/FlorantePascual/strata-protocol-docs/tree/main/docs/strata/3.authority-chain" target="_blank"}
  The constitutional mandate that governs every AI coding session and human contribution on this codebase.
  ::
::

For the complete set — every public stratum document, plus the implementation-template prompts — see the [Artifact Trail index](/case-studies/strataprotocol-org/artifact-trail) and the [Implementation Templates](/case-studies/strataprotocol-org/templates).

## Key Decisions

Three decisions shaped how the platform was built. Each traces back to the Platformization classification and is recorded in the artifact trail.

### Theme — Midnight

The platform ships a single locked visual theme, **midnight** — a violet-and-zinc dark palette chosen for the AI/ML practitioner aesthetic and for consistency across the documentation surface. The theme is fixed in `app/app.config.ts` and protected by a platform-invariant test, so a stray change cannot silently alter the brand. Locking one theme keeps attention on content quality rather than configurability — the right trade-off for a documentation platform.

### The STRATA Visualization (strata.html) Integration

The interactive STRATA architecture diagram was authored as a standalone HTML document rather than rebuilt as native components. It is served as a static asset at `public/strata-visual.html` and embedded through a thin auto-sizing `postMessage` iframe wrapper (`StrataVisual.vue`), exposed to content authors as the `:::strata-visual` MDC component. The standalone file was cleaned from roughly 2,300 to 637 lines — external font references removed, colors aligned to the midnight violet and zinc tokens. This kept a complex, self-contained visualization out of the component build while still letting any Markdown page embed it.

### The RBAC Model

Member content is protected at the server, not just in the browser. Every protected page carries a `requiredRole` in its frontmatter — the client-side route guard — **and** a matching prefix in `PROTECTED_ROUTES`, which `server/middleware/content-guard.ts` enforces on the content API. Frontmatter alone is bypassable by a direct content-API call, so both guards are mandatory; this is the integrity rule this very case-study section follows for its own gated tiers. The role hierarchy is `community → customer → partner → admin`, and the platform runs server-side rendered (never statically generated) precisely so the server guard is always in force.

## Technology Stack

| Concern | Choice |
|---|---|
| Documentation framework | Docus — bundles Nuxt 4, @nuxt/content v3, and Nuxt UI |
| Rendering | Server-side rendering on a Node.js server, containerized with Docker — never static generation |
| Authentication | `nuxt-auth-utils` with sealed, HttpOnly session cookies |
| Password hashing | `bcryptjs` — pure JavaScript, no native bindings |
| Database | `better-sqlite3` at MVP, migrating to PostgreSQL for production deployment |
| Type checking | `vue-tsc` in strict mode |
| Testing | Vitest, with specs co-located beside the utilities they cover |

The stack is deliberately conventional. Platformization rewards reusability and developer experience over novelty, so the platform leans on a well-supported documentation framework and adds only what governance requires — server-side authentication and role-based content protection.

## What This Means for Your Project

The full framework is documented in [The Five Strata](/documentation/the-strata). Read the artifact trail above alongside it: each artifact shows what one stratum produces in practice, on a real project, so you can calibrate the same discipline for your own work.

For the projects that applied these principles before the protocol had a name, see the other [case studies](/case-studies) — Fractal OSI, One Verse Daily, and AI9GM.
