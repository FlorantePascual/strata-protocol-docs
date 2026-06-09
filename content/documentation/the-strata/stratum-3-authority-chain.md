---
title: "Stratum 3 — Authority Chain"
description: The five-node authority chain that constitutionally governs every AI session.
navigation:
  order: 3
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-24"
---

> *The AI is governed, not autonomous. The authority chain defines what the copilot may do and how it must think. It operates inside a mandate — not outside one.*

Stratum 3 — Authority Chain is a five-node constitutional framework that governs every AI copilot session on a STRATA-managed project. It is not a prompt. It is not a style guide. It is a hierarchy of constraints, standards, and mandates — derived from the Stratum 2 document set — that constitutes the copilot's operating authority before the first line of code is generated.

Each node in the chain is derived from the one above it. Nothing in a lower node may contradict a higher one. The chain is versioned alongside the codebase and evolves as the project evolves.

::strata-diagram{name="strata_authority_chain" alt="STRATA Authority Chain — hierarchical funnel of governance documents from Manifesto to the copilot operative constitution"}
::

## Why This Stratum Exists

An AI copilot operating without an authority chain generates against its own training defaults — the last conversation context, its training priors, or nothing at all.

The output may be syntactically correct and functionally plausible. But it is architecturally inconsistent with the project's specific requirements, risk profile, and established standards. It makes assumptions about stack choices that contradict documented decisions. It interprets "clean code" according to training defaults, not the project's engineering standards. It resolves ambiguities silently rather than surfacing them for human judgment.

::warning
A copilot operating without an authority chain is not a governed partner — it is a highly capable guesser. The authority chain converts the copilot from a guesser into a governed actor: one that generates within a defined mandate, escalates ambiguities, and defers architectural judgment to humans.
::

The authority chain eliminates this class of failure by replacing training defaults with a defined, project-specific mandate. The difference between those two states is the difference between AI-assisted development and AI-governed development.

## Key Concepts

### Five-Node Chain Structure

The authority chain is a hierarchy of five documents, each derived from the one above it:

```
Manifesto
  → Vision & Business Case
    → Functional Requirements
      → UI & Testing Standards
        → .github/copilot-instructions.md   ← AI operative constitution
```

**Node 1 — Manifesto**: The project's foundational engineering philosophy. Non-negotiable principles. Written once, updated rarely. Governs everything below it.

**Node 2 — Vision & Business Case**: The strategic framing of the initiative. Governs all requirements, architecture decisions, and scope judgments. Any requirement that cannot trace to the vision is a candidate for removal.

**Node 3 — Functional Requirements**: The numbered, structured definition of what the system must do. Each requirement carries its own analysis document and phased implementation plan. The copilot's execution loop runs against these — one at a time, phase by phase.

**Node 4 — UI & Testing Standards**: Prescriptive standards for how the system looks, behaves, and is verified. The copilot may not deviate from these standards without explicit human instruction.

**Node 5 — `.github/copilot-instructions.md`**: The operative constitution. The single, loadable file that aggregates essential constraints from all upstream nodes into a form the AI can consume at session start.

### The Operative Constitution

`.github/copilot-instructions.md` is the copilot's operative constitution — the single most important artifact in Stratum 3. It has several critical properties:

- **Written in the second person** — speaks directly to the AI ("You are...", "You must...", "You may not...")
- **Imperative throughout** — no suggestions, no preferences, no maybes
- **Self-contained** — a copilot loading this file cold has everything needed to operate correctly on this project
- **Explicit about escalation** — states clearly what the copilot must do when it encounters ambiguity, scope questions, or situations not covered by the chain
- **Versioned** — every material change is committed with a clear change note

The `.github/` location is intentional: GitHub Copilot and most AI-integrated editors load workspace instructions from this path automatically, making the authority chain operative without manual injection every session.

::note
Because `.github/copilot-instructions.md` is loaded automatically by GitHub Copilot, VS Code AI extensions, JetBrains AI Assistant, and Claude Code, the authority chain is a one-time investment that pays per-session. A well-constructed operative constitution requires no manual injection — the tooling loads it before the first token is generated. The only ongoing cost is maintenance: updating the constitution when architecture findings require it.
::

### Governed vs. Autonomous AI

A copilot operating inside an authority chain is a **governed actor**. It generates within a defined mandate, escalates ambiguities rather than resolving them silently, and defers architectural judgment to humans. The scope of its authority is explicit and bounded.

A copilot without an authority chain is an **autonomous actor** — optimized for helpfulness, generating against its own priors, making assumptions that seem reasonable in the moment and compound into architectural inconsistencies over time.

The authority chain does not make the copilot less capable. It makes the copilot's capabilities accountable: every output is generated inside a known mandate, traceable to a document in the chain, and reviewable against defined criteria.

## How Practitioners Apply It

::steps

### Build the chain before the first session

The authority chain is built before the first development session begins — not during development, not retrospectively after problems emerge. A copilot that opens its first session without an authority chain has already generated against training defaults for that session.

### Write the Manifesto

Declare the project's engineering philosophy: what the team believes, how it works, and what it does not compromise on. Register, tone, and principles — not implementation details. This is the highest-authority document in the chain; everything downstream must be consistent with it.

### Reference Stratum 2 documents

Nodes 2 and 3 are derived from the Stratum 2 document set — the Vision & Business Case, BRD, and PRD. These documents do not need to be rewritten for the authority chain; they need to be referenced correctly and held to the chain's authority hierarchy.

### Write the UI & Testing Standards

Prescriptive standards — not style preferences. These standards govern how all code on this project is written, how components are structured, how tests are co-located, what coverage is required, and what patterns are prohibited. The copilot must not deviate from these standards without explicit human instruction.

### Write the operative constitution

Aggregate essential constraints from all four upstream nodes into `.github/copilot-instructions.md`. Structure it with: identity and mandate, project context, authority chain reference, stack constraints, code generation rules, testing requirements, output format requirements, escalation protocol, and prohibited actions. Write it in the second person, imperative throughout, self-contained.

### Version and maintain the chain

Commit the constitution to the repository. Track every material change in an authority chain version log. When an architecture finding logged during the Execution Loop identifies an upstream implication, update the constitution in the same commit that addressed the finding — do not defer.

::

## What Goes Wrong Without It

**Authority chain drift** — the most insidious failure mode in AI-assisted development.

Early sessions may be coherent because the copilot has recent context. As the project grows and context windows rotate across sessions, the copilot's effective mandate diverges from the project's actual requirements. Each session produces output that is architecturally inconsistent with prior sessions. The codebase accumulates contradictions that compound with every new phase.

Specific consequences:
- A pattern established in Phase 2 is "corrected" in Phase 7 by a session that does not know why it was established
- Stack constraints documented in the architecture are ignored because they were not in the last session's context
- Escalation triggers are missed because the copilot resolved ambiguities silently that it should have surfaced
- Each new session's output requires more human review to catch inconsistencies that a functioning authority chain would have prevented

By the time the drift is apparent, it has accumulated across many phases. Resolving it requires a systematic audit of all generated code against the authority chain that should have governed it from the start — expensive work that the chain itself would have made unnecessary.

## StrataProtocol.org Example

`.github/copilot-instructions.md` is the operative constitution for this platform.

It establishes the copilot's identity (governed development copilot for a Nuxt 4/Docus documentation portal with SSR authentication and RBAC), project context (Class 1, Category 1.3 — Platformization), and a comprehensive stack constraints table. Critical sections include:

- **RBAC integrity rule**: Four-step checklist for any page with `requiredRole` in its frontmatter — frontmatter, `PROTECTED_ROUTES`, test case, and passing unit tests — all required in the same generation pass
- **Never-do list**: Eight explicit prohibitions, including never adding `nuxt generate` (which would expose member content to unauthenticated users) and never modifying `content-guard.ts` without explicit human instruction
- **Escalation protocol**: Six trigger conditions for stopping and escalating to the human engineer before proceeding

Every session that generated code on this platform — from the platform bootstrap through every content requirement — operated inside this chain. The authority chain version log in `docs/strata/3.authority-chain/` records every material change and the reason for it.

::u-page-c-t-a
---
title: Go deeper with a member account
description: The Stratum 3 deep-dive covers the complete Authority Chain templates for all five nodes, the Derivation Agent prompt for authority chain construction, worked examples across different project types, and the version log convention.
variant: soft
links:
  - label: Access with a member account
    to: /documentation/deep-dives/
    icon: i-ph-lock
    color: neutral
---
::

## Related Strata

**← Previous:** [Stratum 2 — Derivation](/documentation/the-strata/stratum-2-derivation) — provides the 14 normative documents that the authority chain is derived from. The Vision & Business Case, Functional Requirements, and Engineering Standards are the upstream inputs to Nodes 2, 3, and 4 of the chain.

**→ Next:** [Stratum 4 — Execution Loop](/documentation/the-strata/stratum-4-execution-loop) — the Execution Loop runs inside the mandate established by the authority chain. Every sprint session opens by loading the operative constitution; every gate decision is made by the human who is the gate this chain designates.
