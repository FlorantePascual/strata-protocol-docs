---
title: "Stratum 1 — Classification"
description: Classify before you specify. The project type determines every downstream engineering decision.
navigation:
  order: 1
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-24"
---

> *Classify before you specify. The project type determines everything downstream.*

Stratum 1 — Classification is the first act of technical authority in the STRATA Protocol. Before any specification is written, before any architecture decision is made, before any AI session is opened, the project is classified by business intent, operational constraints, and continuity requirements.

Technology choices are implementation details. Business objectives determine the appropriate engineering strategy. The Classification Engine produces a single output: a signed **Classification Record** — the first document in the project's authority chain, and the input to every subsequent stratum.

::strata-diagram{name="strata_classification_engine" alt="STRATA Classification Engine — decision tree showing the two primary classes and six categories of project classification"}
::

## Why This Stratum Exists

Software projects fail when they are built at the wrong risk tolerance.

A Greenfield MVP built with the engineering rigor appropriate for an Established System wastes months optimizing for constraints that do not yet exist. An Established System rebuilt with the informality acceptable in an MVP destroys existing customer value and operational continuity. Both failures share the same root cause: the project was not classified before the first decision was made.

::warning
When classification is wrong, everything downstream is built on a false premise. Specifications are written at the wrong depth. Architecture decisions are made at the wrong risk tolerance. AI sessions operate without the right constraints. Delivery teams optimize for the wrong outcomes.
::

Classification prevents this category of failure before it can occur. By answering "What kind of project is this?" before any specification is written, STRATA ensures that all downstream decisions — architecture depth, document depth, migration strategy, testing rigor — are calibrated to the actual engineering situation, not a misread of it.

## Key Concepts

### Classify Before You Specify

The foundational principle of Stratum 1: before a single requirement is written, a single architecture decision is made, or a single AI session is opened, the project must be classified. The classification question is always: *"What business outcome is this initiative intended to achieve?"* — not "What technology are we using?" or "What are we building technically?"

Projects are commonly misdescribed in implementation terms — "We are rewriting the backend" or "We are moving to microservices." These describe implementation decisions, not business intent. They answer *how* before anyone has agreed on *why*. Classification forces the *why* to be answered first.

### Two Primary Classes

All software initiatives fall into one of two primary classes:

| Class | Core business reality |
|---|---|
| **Class 1 — Startup / MVP Systems** | The business is still discovering or validating value |
| **Class 2 — Established Systems** | The business has proven operational value that must be preserved while the system evolves |

The distinction is not about company size. It is about continuity constraints, operational risk, and business maturity.

**Class 1** optimizes for learning and market validation. Requirements are fluid. Architecture is secondary to iteration speed. Technical debt is acceptable if it accelerates learning. The system exists primarily to test business hypotheses.

**Class 2** optimizes for safe change. Migration risk matters. Customer trust matters. Architecture decisions are business continuity decisions. The objective is not only building software — it is changing the system without damaging existing business value.

::tip
**Key insight:** The class boundary is determined by continuity, not scale. A solo founder with a hundred paying customers is operating a Class 2 system — because those customers depend on its stability. A large enterprise team building an internal prototype is operating a Class 1 system. Company size, team size, and technical complexity are not classification criteria. Only one question matters: does existing business value depend on this system continuing to work?
::

### Six Categories

Each class contains three categories. The category is determined by the specific business objective within the class:

| Class | Category | Primary objective |
|---|---|---|
| Class 1 | 1.1 Greenfield MVP | Discover and validate value |
| Class 1 | 1.2 Expansionary development | Extend business capability |
| Class 1 | 1.3 Platformization | Create leverage and reusable capability |
| Class 2 | 2.1 Transitional rewrite | Preserve behavior while replacing implementation |
| Class 2 | 2.2 Evolutionary rebuild | Preserve business while redefining the system |
| Class 2 | 2.3 Modernization | Improve the system incrementally while operating continuously |

### Continuity Constraints

The more continuity constraints that exist — paying customers, operational workflows, third-party integrations, public APIs, compliance obligations, SLAs, revenue reliance — the more the engineering strategy must account for preservation, not just construction. Continuity constraints are the primary driver of Class 2 depth requirements and the reason Layer 6 (Migration Strategy) is treated as critical for Established Systems.

## How Practitioners Apply It

::steps

### Answer three questions in sequence

**Question 1 — Is the business still validating value?**
If yes: Class 1. If no: Class 2. This is the primary classification gate. Answer it honestly before moving to the next question.

**Question 2 — What continuity constraints already exist?**
Consider: paying customers, operational workflows, third-party integrations, public APIs, compliance obligations, SLAs, revenue reliance. Inventory them. The more that exist, the more the engineering strategy must account for preservation. A Class 1 project with early paying customers is approaching a reclassification trigger.

**Question 3 — Is the business model changing?**
Match the situation to the six categories. New capability discovery → Greenfield MVP. Capability expansion → Expansionary development. Infrastructure leverage → Platformization. Same product, new implementation → Transitional rewrite. Evolving business model → Evolutionary rebuild. Incremental operational improvement → Modernization.

### Produce the Classification Record

Document the classification: class, category, primary business objective (in business terms, not implementation terms), continuity constraints, rationale for the classification, and the risks of misclassification. Sign it. Commit it to the repository. This becomes the first document in the authority chain and the first input to Stratum 2.

### Hold the classification stable

Classification is not revised because a project is harder than expected, or because a new technical preference emerges. It is revised only if a material business-level change occurs — for example, a Class 1 project that acquires paying customers mid-build crosses into Class 2 territory. That is a formal reclassification event: document it, assess downstream implications, and revise the derivation documents accordingly.

::

## What Goes Wrong Without It

The most common failure mode: treating a Class 2 — Established System as a Class 1 Greenfield MVP.

The team moves fast, accepts technical debt, skips migration planning, and makes architecture decisions without rationale. Progress feels rapid. When customers begin experiencing disruptions — data inconsistencies during the migration, broken integrations, degraded performance — the team discovers that the "fast" approach has accumulated continuity risk that now costs far more to resolve than a correct initial classification would have cost to establish.

The inverse failure — treating a Class 1 project as Class 2 — overbuilds the system: excessive migration planning for a product that has no live users, NFR suites for performance characteristics that will never be reached, architecture designed for compliance obligations that do not yet exist.

Neither failure is the result of incompetence. Both are the result of absent classification.

## StrataProtocol.org Example

This platform is classified as **Class 1, Category 1.3 — Platformization**.

The STRATA methodology was an internally proven engineering practice before this platform was built. The initiative converts that validated internal capability into an accessible, scalable, externally-facing platform — exactly the Platformization pattern: a methodology becomes a documentation platform, internal templates become a member toolkit, a personal engineering practice becomes a framework accessible to practitioners worldwide.

This classification shaped every engineering decision:

- **Content architecture**: Must generalize across project types (Platformization engineering priority: Abstraction)
- **Template depth**: Must be actionable, not merely illustrative (Platformization engineering priority: Developer experience)
- **Documentation quality**: Must be defensible against the framework it describes — the self-referential proof (Platformization engineering priority: Reusability)

A misclassification as Greenfield MVP (1.1) would have permitted thinner content architecture, rough navigation, and placeholder-heavy documentation. For a methodology platform that exists to demonstrate best-in-class practice, those are not acceptable trade-offs — they are contradictions.

The full Classification Record is available in the platform's artifact trail at `docs/strata/1.classification/classification-record.md`.

::u-page-c-t-a
---
title: Go deeper with a member account
description: The Stratum 1 deep-dive covers the complete Classification Engine with worked examples across all six categories, decision frameworks for boundary cases, and the implementation template for producing a signed Classification Record.
variant: soft
links:
  - label: Access with a member account
    to: /documentation/deep-dives/
    icon: i-ph-lock
    color: neutral
---
::

## Related Strata

Stratum 1 is the first stratum — nothing precedes it in the STRATA Protocol.

**→ Next:** [Stratum 2 — Derivation](/documentation/the-strata/stratum-2-derivation) — consumes the Classification Record as its first and highest-authority input. Document depth, risk weighting, and which of the 14 normative documents are required versus stubbed are all determined by the Classification.
