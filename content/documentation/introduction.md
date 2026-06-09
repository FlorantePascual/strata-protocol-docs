---
title: Introduction
description: What STRATA is, why it exists, and the governance problem it solves.
navigation:
  order: 1
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-24"
---

## The Governance Gap

AI-assisted development is powerful. It can generate code, architecture documentation, test cases, and entire feature implementations in minutes. But power without governance creates risk: hallucinated requirements, inconsistent patterns, decisions made at the speed of autocomplete, and systems built on assumptions no human engineer verified.

The standard responses — "review the AI output carefully" or "use the AI as a starting point" — place the burden of governance entirely on human attention and review discipline. They do not prevent bad decisions. They only catch them after they are made.

::warning
The governance gap is the absence of a structured protocol that makes the AI a governed partner in development — not a code generation service, but a thinking agent that operates inside a clear mandate.
::

## What STRATA Is

STRATA is a five-stratum governance framework for AI-assisted software delivery. It establishes a complete protocol from project classification through artifact preservation, ensuring that every decision made during development is made against a defined authority, that every phase of work produces verifiable output, and that the entire journey is documented in a way that makes future maintenance and extension possible.

The five strata are:

- **Stratum 1 — Classification:** Project type determines everything downstream. You classify projects by business intent and continuity constraints before any specification is written.

- **Stratum 2 — Derivation:** Before you can build a system, you must know what it should do. The derivation stratum produces a complete set of normative documents — vision, requirements, architecture, standards, risk analysis — derived in dependency order.

- **Stratum 3 — Authority Chain:** A constitutional framework that governs the AI copilot for the entire development session. The authority chain is a hierarchy of constraints: manifesto, vision, requirements, standards, and operative constitution. Nothing the AI does can contradict anything upstream.

- **Stratum 4 — Execution Loop:** The structured cycle through which human and AI collaborate, phase by phase. Each phase is scoped, deliverables are defined, acceptance criteria are stated, and the human engineer is the gate before any phase advances. The loop makes AI-assisted development trustworthy.

- **Stratum 5 — Artifact Trail:** The living record that outlives the project. Every decision, deviation, gate pass, and context transition is preserved in a curated, structured artifact trail. Future engineers — or future AI sessions — have high-fidelity context, not scattered notes or undocumented code.

Together, these five strata transform AI from a capability to a controlled, governed partnership.

::tip
**Key insight:** The five strata are sequential and dependent. Each stratum establishes the foundation for the next. Stratum 1 answers "what are we building?" before Stratum 2 asks "what do we need to know?" This dependency chain is what makes the entire framework trustworthy.
::

## Who STRATA Is For

**STRATA is for anyone building software with AI.**

::note
STRATA is language-agnostic, framework-agnostic, and tool-agnostic. Whether you use Python or TypeScript, React or Vue, GPT-4 or Claude, the governance structure applies. The framework is about decision-making discipline, not technology choices.
::

This includes:

- **Tech leads and architects** who need a framework to govern AI-assisted development without slowing it down
- **Founders and startup engineers** who need to move fast without sacrificing governance or creating technical debt through unmeasured AI decisions
- **Enterprise development teams** who need to ensure that AI-assisted work meets the same standards as human-led development
- **Independent practitioners** who want a systematic approach to AI-assisted projects
- **Organizations evaluating AI adoption** for custom software development and needing a governance model

## Proof: StrataProtocol.org Is Built Using STRATA

::tip
This section demonstrates a self-referential proof: StrataProtocol.org was built *by applying the STRATA Protocol itself*. Not retrospectively documented, but actively governed from day one. The platform is both the message and the medium.
::

This platform — StrataProtocol.org — is itself the first public case study of STRATA in operation.

The platform was classified as a **Class 1 — Platformization** project: a startup-phase initiative with the strategic objective of creating reusable capability — the STRATA framework as a published, accessible protocol.

Before any code was written, the project was derived into complete specifications — a vision and business case that established why the platform exists, business and product requirements that defined what it must do, and target architecture that defined how it is built.

The AI copilot (GitHub Copilot) is governed by a constitutional framework — an authority chain that establishes what the copilot may do, what patterns it must follow, and what architectural decisions it cannot deviate from without explicit human instruction.

Each feature is implemented in phases, with each phase running through the Execution Loop. The human engineer is the gate — no phase advances without sign-off. Every deviation from the plan is logged. Every decision that deviates from upstream authority is documented and justified.

The entire journey is recorded in an artifact trail — the complete documentation of how the platform came to be, why each decision was made, and how the AI was governed throughout.

This is not a retrospective documentation of a project that was built in ad-hoc fashion. This is a platform built by applying the STRATA Protocol itself — a self-referential proof that the framework works in practice.

## Next Steps

- **Learn more:** [Getting Started](/documentation/getting-started) — how to apply STRATA to your projects
- **Explore the five strata:** [The Five Strata](/documentation/the-strata) — read individual narratives for each stratum
- **Understand the principles:** [Core Concepts](/documentation/concepts) — governing philosophy and design principles
- **Reference:** [Glossary](/documentation/reference) — complete terminology and definitions
