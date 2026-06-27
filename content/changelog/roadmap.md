---
title: Platform Roadmap
description: Where StrataProtocol.org stands today — what is live, what is actively being built, and what is planned next.
status: stable
navigation:
  order: 2
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-06-26"
---

:status-badge{status="stable"}

The current state of every section of StrataProtocol.org — what is live and complete, what is actively expanding, and what is planned. This page is the canonical source of truth for platform completeness; individual sections carry a matching `StatusBadge` for quick reference.

## Available Now

Everything listed here is live and complete for public or member access today.

**Core Documentation** — public
- [Introduction](/documentation/introduction) — What STRATA is, the problem it solves, and who it is for.
- [Getting Started](/documentation/getting-started) — How to apply STRATA on your first project.
- [The Five Strata](/documentation/the-strata) — Complete narrative for all five strata: Classification, Derivation, Authority Chain, Execution Loop, Artifact Trail.
- [Core Concepts](/documentation/concepts) — The governing principles that run through all five strata.
- [Reference Glossary](/documentation/reference/full-glossary) — Full canonical definitions for every key term.
- [Cite the Framework](/documentation/citation) — APA and BibTeX citation strings with the live Zenodo DOI.
- [Developer Toolkit](/documentation/developer-toolkit) — Public preview of the member Developer Guide: platform architecture, governed delivery workflow, and implementation-flow diagrams.

**Deep Dives** — member access (customer)
- [Stratum 1 — Classification Deep Dive](/documentation/deep-dives/stratum-1-classification)
- [Stratum 2 — Derivation Deep Dive](/documentation/deep-dives/stratum-2-derivation)
- [Stratum 3 — Authority Chain Deep Dive](/documentation/deep-dives/stratum-3-authority-chain)
- [Stratum 4 — Execution Loop Deep Dive](/documentation/deep-dives/stratum-4-execution-loop)
- [Stratum 5 — Artifact Trail Deep Dive](/documentation/deep-dives/stratum-5-artifact-trail)

**Developer Guide — Implementation Templates** — public funnel + member access (customer)
- Public funnel: [Introduction](/developer-guide/introduction) and the [Developer Toolkit](/documentation/developer-toolkit) preview.
- Member templates: complete Nuxt/Docus, NestJS Backend, and Vue Frontend feature templates, plus getting-started, architecture, and reference pages.

**AI Playbook** — public + member access (customer)
- [Introduction](/ai-playbook/introduction) and [Core Concepts](/ai-playbook/concepts) — public.
- Thirteen member governance prompts — a guided prompt set for each of the five strata.

**Case Studies** — public + member access (customer / partner)
- [StrataProtocol.org](/case-studies/strataprotocol-org) — the public self-referential proof: overview, artifact trail, and template index linking to the platform's own governed records.
- Public summaries for [Fractal OSI](/case-studies/fractal-osi), [One Verse Daily](/case-studies/one-verse-daily), and [AI9GM](/case-studies/ai9gm), each with a member-gated full study (customer) and development journal (partner).

**Engage** — public
- [Services & Engagement](/engage) and the [qualification flow](/apply) — member access granted through a free discovery call.

**Platform Infrastructure** — complete
- Authentication, membership, and admin user management with full audit logging.
- Dual-layer content protection: browser route guard + server-side Content API guard.
- PostgreSQL database on a stateless application container; production Docker deployment stack.
- Legal framework: Terms, Privacy, Cookie, Acceptable Use, and Refund & Billing policies (linked in the footer).
- Self-referential governance artifact trail in `docs/strata/`, published publicly via [Case Study #1](/case-studies/strataprotocol-org).

---

## In Progress

Sections rolling out incrementally — partial value available today, more being added actively.

**Case Studies — depth** — early access
- Case #1 (StrataProtocol.org) is complete and public; the three third-party studies — summaries public, full studies and journals member-gated — carry an Early Access badge as their depth expands.

**Marketplace** — expanding
- Partner directory structure live: Implementation Partners, Advisory Services, Templates, and Developer Tools categories.
- Transactional listing system and premium tier planned in a future release.

**[Platform Updates](/changelog/updates)** — early access
- Section live; release notes, engineering notes, and practitioner stories being published incrementally.

---

## Planned

Not yet started — on the confirmed roadmap.

**Video Courses** — planned
- Five-stratum video walkthrough series.
- Applied sessions with GitHub Copilot, Cursor, and Claude in governed STRATA environments.
- See [Video Courses](/video-courses) for the planned curriculum.

**Marketplace — Premium Tier** — planned
- Partner account system and verified listing management.
- `/marketplace/premium` reservation in place; full transactional system is a future release.

---

## Recently Shipped

**2026-06-25 — REQ-013: Self-Referential Artifact Trail**
The Case Studies section is live — StrataProtocol.org's own construction published as a public, navigable self-referential proof, plus three pre-STRATA case studies on a tiered proof-asset ladder (public summary → member full study → partner journal).

**2026-06-24 — REQ-011: Documentation Platform Quality**
Foundational operating rules locked as CI-enforced invariants (SSR-only, bundled dependencies, midnight theme); performance and font posture verified; mobile rendering polished.

**2026-06-17 — REQ-020: Legal Framework**
Five legal policies, clickwrap consent on registration and login, and a footer legal-links row — all from a single configuration source, zero new dependencies.

**2026-06-09 — REQ-012: PostgreSQL Migration & Docker Deployment**
Database pivoted from SQLite to PostgreSQL on a stateless application container, with a production Docker stack.

**2026-06-08 — REQ-017: Authorship & SEO Identity**
Verifiable authorship and structured SEO metadata (WebSite, TechArticle, and Person JSON-LD) across every page, plus an `/about` identity hub.

**2026-06-05 — REQ-014: Qualified Engagement**
Subscription billing replaced with a qualified-access consulting model: member access granted through a free discovery call.

See [Changelog](/changelog) for the full version history.

::u-page-c-t-a
---
title: Access the full member toolkit
description: Deep-dive documentation, implementation templates, and guided prompt sets for all five strata — available with a member account.
variant: soft
links:
  - label: Explore member resources
    to: /documentation/deep-dives
    trailingIcon: i-ph-arrow-right
---
::
