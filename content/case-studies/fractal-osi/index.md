---
title: Overview
description: Case Study — Fractal OSI, a Class 1.2 project built on VueJS, NestJS, and an n8n AI workflow. An early adoption of the full five-stratum STRATA sequence, predating the protocol's formal specification.
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

Fractal OSI (fractalosi.com) is the Ontological Semantic Information Modeler — a visual platform for building formally typed, semantically rich models of any domain. Where an ordinary diagramming tool draws boxes and arrows that mean whatever the viewer assumes, Fractal OSI gives every object a formal ontological type and every relationship a defined semantic class, then enforces those rules as you build. Models nest recursively — any model can become a component inside a larger one — which is where the name *Fractal* comes from. It grew well past a canvas: multi-tenant teams, authentication, pluggable storage providers, digital assets, AI-assisted schema generation, and an embeddable live canvas.

It is also the project on which STRATA was discovered by doing. Long before the protocol had a name, Fractal OSI was already being delivered through a document-first, phase-gated, AI-assisted discipline — and the recurring shape of that practice is what later became the five strata.

## Classification: Class 1.2 — Expansionary

Fractal OSI is classified **Class 1, Category 1.2 — Expansionary**. Class 1 because the product was still discovering and validating its market form. Expansionary rather than Greenfield MVP (1.1) because it was not a single unproven hypothesis: it built a new, multi-tenant product on an established conceptual foundation — formal ontological modeling — and expanded outward from a working canvas into a full platform surface. That classification set the engineering priorities: architectural integrity across the meta-model → model → instance layers, and a quality gate strict enough to keep a fast-growing surface coherent.

## Technology Stack

| Concern | Choice |
|---|---|
| Frontend | VueJS single-page app — Vue 3, Vite, Pinia, and Vue Flow driving the canvas engine |
| Backend | NestJS — a typed API contract for auth, teams, providers, assets, billing, and audit |
| AI workflow | n8n — workflow automation orchestrating the AI-assisted modeling features |
| Language / tests | TypeScript in strict mode; Vitest with co-located specs |

## STRATA Principles Observed

Fractal OSI ran all five strata by hand before they had names:

- **Classification** — an implicit "startup, speed-primary, change-tolerant" stance that scoped each feature's ambition before any work began.
- **Derivation** — every feature's analysis and implementation plan derived from fixed templates, in dependency order, against a global glossary and folder-structure map.
- **Authority chain** — a "Working Constitution" plus testing and UI standards that constrained what the AI assistant was allowed to generate — the literal precursor to STRATA's operative constitution.
- **Execution loop** — a per-phase build loop with implementation manifests, human sign-off between phases, and dedicated unplanned-scope notes so deviations were fed forward rather than lost.
- **Artifact trail** — running commit logs, phase manifests, deviation records, and a conventional-commit history that threads each feature's identifier through every commit.

::tip
The methodology was not designed and then applied here — it was extracted from here. Across dozens of feature releases the same five layers kept producing the cleanest, most traceable results, and the per-feature prompt files show the discipline visibly sharpening release by release. STRATA is the write-up of what already worked on Fractal OSI.
::

## What STRATA Would Have Formalized

Almost everything Fractal OSI did by convention, STRATA makes explicit. The implicit MVP stance would be a written Classification Record with a documented reclassification trigger. The template-derived per-feature documents would descend from a numbered, governed product-requirements set. The Working Constitution would be a named authority chain subordinate to a Vision and Business Requirements document. The practice was already sound; STRATA gives it a constitution, so the next project inherits the discipline instead of rediscovering it. See [The Five Strata](/documentation/the-strata) for the formalized version, and the [Authority Chain](/documentation/the-strata/stratum-3-authority-chain) for the stratum this project's Working Constitution anticipated.

::u-page-c-t-a
---
title: Go deeper than the summary
description: The full five-stratum walkthrough of Fractal OSI — every layer, every decision — plus the raw development journal behind the build.
variant: soft
links:
  - label: Read the full case study
    to: /case-studies/fractal-osi/full
    icon: i-ph-lock
    color: primary
  - label: Open the development journal
    to: /case-studies/fractal-osi/journal
    icon: i-ph-notebook
    color: neutral
---
::
