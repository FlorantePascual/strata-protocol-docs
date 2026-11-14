---
title: Overview
description: "Case Study. AI9GM, Class 1.3. Publication site shipped on Astro under STRATA. Early full five-stratum AI-assisted delivery. Differentiator: decision rights across 94 permanent decisions. MVP velocity: 117 commits in five weeks from 22 August 2026"
status: early-access
navigation:
  order: 0
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-09-29"
---

:status-badge{status="early-access"}

AI9GM ([ai9gm.com](https://ai9gm.com)) is the AI-Centric IT Governance Model. It is an open
reference model for governing AI across the estate an AI system depends on. It organizes
governance into six interdependent layers (Foundation, Structural, Intelligence, Control, Execution
and Strategic). It extends established standards such as COBIT, ITIL and ISO into the AI era. It
does not replace them.

**Decision rights** set the published model apart. AI9GM names **94 decisions** with permanent
identifiers. For each decision it states who Decides, who is Consulted, who Executes and what
Evidence must be retained. Standards often leave that allocation implicit. AI9GM makes it the
primary reading surface. Layer 4 (Control) holds a large share of the catalog. Every layer still
carries its own rights.

AI9GM and StrataProtocol.org were developed in parallel and informed each other. An early shared
bet was a Docus-style documentation portal. StrataProtocol.org shipped on **Nuxt and Docus**. The
AI9GM publication site later shipped on **Astro 7** (Lotus theme) under the same STRATA delivery
discipline. The parallel is methodological. It is not a claim that both products share one
frontend stack today.

## Classification: Class 1.3 Platformization

AI9GM is classified **Class 1, Category 1.3 Platformization**, the same category as
StrataProtocol.org. The work did not invent an unproven product. It took a governance model
refined through practice and converted it into an accessible, externally facing standard.

That is the Platformization move: a validated capability becomes a public, reusable platform. Here
a governance model becomes a versioned, citable framework with permanent addresses for its
decisions.

## Technology Stack

| Concern | Choice |
|---|---|
| AI9GM publication site | Astro 7 on a Node runtime, Vue islands where needed, Lotus theme, PostgreSQL for write-path state |
| StrataProtocol.org | Nuxt with Docus (@nuxt/content and Nuxt UI) |
| Shared discipline | STRATA Protocol: document first, gate each phase, preserve the artifact trail |

## STRATA Principles Observed

AI9GM is unusual among these cases. Its subject is layered governance, so the parallel with STRATA
is structural, not only methodological.

- **A layered governance model.** AI9GM's six interdependent layers mirror STRATA's stratum model.
  Governance fails when its concerns are tangled. It holds when they are layered so each builds on
  the one beneath it.
- **Decision rights as the binding surface.** Permanent identifiers and an explicit Decides /
  Consulted / Executes / Evidence allocation turn a layer diagram into something an organization
  can operate and correct against.
- **Self-documenting by design.** The governance framework for AI is published under STRATA to a
  high documentary standard. The live site is both the model and the record of how it is structured.
- **Honest maturity.** The published site states what v0.9 is and what it has not earned
  (including certification and community governance). That refusal is part of the proof.

## Delivery velocity under STRATA

The AI9GM publication MVP ran under STRATA from **22 August 2026**. In **five weeks** the repository
took **117 commits**. That count includes engineering work and the operational work the release
required: DOI registration for the archival record and Coolify deployment of the Node host. The
point is not the commit total as vanity. It is that a governed AI-assisted loop carried a Class 1
site from adopted specification through permanence, write path, discoverability and production
acceptance without inventing a second methodology mid-flight. STRATA Protocol works when the
artifact trail and the human gate stay intact.

::tip
AI9GM and STRATA arrived at the same conviction from opposite ends. AI9GM governs AI at the
enterprise level (infrastructure, data, risk, accountability, strategy). STRATA governs AI at the
engineering level (how a single feature is specified, built and traced). Two frameworks, developed
in parallel, independently treat ungoverned AI as a liability. That convergence is evidence the
governance gap is real.
::

## What STRATA Would Have Formalized

AI9GM was self-documenting by design. Much of its early construction discipline lived in practice
rather than in a named protocol. STRATA makes that explicit for later work: a written Classification
Record fixing the platformization stance, a derivation chain from strategic vision down through the
governance layers and a five-stratum artifact trail for how the framework is published, not only
what it prescribes. The model already taught governance. STRATA now governs the publication
delivery the same way this platform does. See the
[Execution Loop](/documentation/the-strata/stratum-4-execution-loop).

::u-page-c-t-a
---
title: Go deeper than the summary
description: Full five-stratum walkthrough of AI9GM, decision rights and the development journal.
variant: soft
links:
  - label: Read the full case study
    to: /case-studies/ai9gm/full
    icon: i-ph-lock
    color: primary
  - label: Open the development journal
    to: /case-studies/ai9gm/journal
    icon: i-ph-notebook
    color: neutral
  - label: Decision rights on ai9gm.com
    to: https://ai9gm.com/the-model/decision-rights/
    icon: i-ph-arrow-square-out
    color: neutral
    target: _blank
---
::
