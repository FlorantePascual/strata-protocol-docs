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
date: "2026-05-27"
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
- [Developer Toolkit](/documentation/developer-toolkit) — Public preview of the member Developer Guide: platform architecture, governed delivery workflow, and implementation-flow diagrams.

**Deep Dives** — member access (customer)
- [Stratum 1 — Classification Deep Dive](/documentation/deep-dives/stratum-1-classification)
- [Stratum 2 — Derivation Deep Dive](/documentation/deep-dives/stratum-2-derivation)
- [Stratum 3 — Authority Chain Deep Dive](/documentation/deep-dives/stratum-3-authority-chain)
- [Stratum 4 — Execution Loop Deep Dive](/documentation/deep-dives/stratum-4-execution-loop)
- [Stratum 5 — Artifact Trail Deep Dive](/documentation/deep-dives/stratum-5-artifact-trail)

**Platform Infrastructure** — complete
- Authentication system: secure sessions, invitation-based member provisioning.
- Dual-layer content protection: browser route guard + server-side Content API guard.
- Self-referential governance artifact trail in `docs/strata/`.

---

## In Progress

Sections rolling out incrementally — partial value available today, more being added actively.

**AI Playbook** — in progress
- [Introduction](/ai-playbook/introduction) and [Core Concepts](/ai-playbook/concepts) available now.
- Member-only guided prompt sets for each stratum: in development (REQ-007).

**Case Studies** — early access
- Section structure and four project profiles scaffolded: [StrataProtocol.org](/case-studies/strataprotocol-org), [AI9GM](/case-studies/ai9gm), [Fractal OSI](/case-studies/fractal-osi), [One Verse Daily](/case-studies/one-verse-daily).
- Full narrative write-ups in progress (REQ-013).

**Marketplace** — expanding
- Partner directory structure live; Implementation Partner, Advisory Services, Templates, and Developer Tools categories available.
- Transactional listing system and premium tier planned in a future release.

**[Platform Updates](/changelog/updates)** — early access
- Section live; release notes, engineering notes, and practitioner stories being published incrementally.

---

## Planned

Not yet started — on the confirmed roadmap.

**Developer Guide** — planned (REQ-006)
- Full implementation template library: Nuxt/Docus feature templates, NestJS/Vue patterns, and additional stacks.
- Guided workflows: step-by-step worked examples for each stratum.
- Reference architecture with annotated decision records.
- Preview available now on the public [Developer Toolkit](/documentation/developer-toolkit) page.

**Video Courses** — planned
- Five-stratum video walkthrough series.
- Applied sessions with GitHub Copilot, Cursor, and Claude in governed STRATA environments.
- See [Video Courses](/video-courses) for the planned curriculum.

**Marketplace — Premium Tier** — planned
- Partner account system and verified listing management.
- `/marketplace/premium` reservation in place; full transactional system is a future release.

---

## Recently Shipped

**2026-05-27 — REQ-005C: WIP Messaging & Progressive Launch**
Converted all incomplete section placeholders into intentional, value-bearing previews. Added `StatusBadge` component and this Roadmap page. Platform is now fully public-launch ready.

**2026-05-27 — REQ-005B: Strata SVG Diagrams, Light Mode, Font Fix**
All five STRATA diagrams shipped as adaptive single-file SVGs (light + dark mode). Light-mode contrast and font-stack issues resolved across the platform.

**2026-05-27 — REQ-005: Member Deep-Dive Documentation**
All five stratum deep-dives published for member access — complete procedural guides from classification through artifact preservation.

**2026-05-19 — v0.1.0: Initial Platform Launch**
Complete Nuxt 4 / Docus documentation portal. Five-stratum public documentation structure. Authentication, dual-layer RBAC, and self-referential governance artifacts.

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
