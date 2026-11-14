# Stratum 2 — Derivation

Documents are derived from business intent, not written from instinct. Each document answers exactly one framing question.

*Rendered from [strataprotocol.org/documentation/the-strata/stratum-2-derivation](https://strataprotocol.org/documentation/the-strata/stratum-2-derivation) at STRATA Protocol release 1.2.0.*

> *Documents are derived, not written. Every artifact answers exactly one framing question. If you cannot state the question, you are not ready to produce the document.*

Stratum 2 — Derivation is the knowledge construction engine of STRATA. It takes the Classification Record produced in Stratum 1 and converts it into a complete, challenged, and finalized set of normative documents — the foundational data from which the Authority Chain (Stratum 3) and Execution Loop (Stratum 4) operate.

Stratum 1 answers: *What kind of project is this?* Stratum 2 answers: *What do we need to know before we build it — and in what order do we need to know it?*

![STRATA Derivation Loop — four-stage cycle of Frame, Derive, Challenge, and Finalize for producing normative documents](https://strataprotocol.org/lib/diagrams/strata_derivation_loop.svg)

## Why This Stratum Exists

Without a structured derivation process, documents are written from instinct rather than derived from intent.

Requirements contradict the vision because no one checked. Architecture decisions are made without rationale because no ADR was written. Testing standards are aspirational rather than achievable because they were never challenged against the architecture they are supposed to verify. The document set is internally inconsistent, and every AI session that generates against it compounds the inconsistency.

Derivation enforces dependency order on understanding. You cannot answer *how* the system will be built before you know *what* it must do. You cannot plan a migration before you have a target architecture. The 7-layer mental model makes this dependency order explicit, names it, and requires it to be followed — not as a bureaucratic formality, but because understanding truly is sequential.

> [!NOTE]
> A document that cannot be challenged against the documents above it in the dependency chain is not finalized — it is drafted. Finalization is a deliberate act, not a passive completion. In STRATA, only challenged and finalized documents are normative inputs to subsequent layers.

## Key Concepts

### The 7-Layer Mental Model

The thinking sequence that enforces dependency order on all Stratum 2 work. Each layer answers one fundamental question and produces one category of document artifact. Layers must be worked in sequence — a lower layer that contradicts a higher one signals an incomplete or incorrect higher layer, requiring a return to it before proceeding.

| Layer | Dimension | Question | Document category |
|---|---|---|---|
| 1 | Why | Why are we doing this? | Vision & Business Case |
| 2 | What | What should the system do? | Requirements (BRD + PRD) |
| 3 | How — Functional | How will users interact with it? | Functional Specifications |
| 4 | How — Technical | How will it be built? | Architecture |
| 5 | How — Quality | How do we ensure robustness? | NFR & Quality Plan |
| 6 | How — Transition | How do we transition safely, in or out? | Transition Strategy |
| 7 | How — Execution | How do we deliver? | Roadmap & Delivery Plan |

Layers 1–2 are the *strategic* layers — business language, no implementation detail. Layers 3–5 are the *design* layers — bridging business intent and engineering behavior. Layers 6–7 are the *delivery* layers — continuity, risk, and sequencing.

### Layer 6 Runs Both Ways

Layer 6 is Transition, and transition runs in two directions. **Inbound** is migration: moving off a predecessor system without damaging the business, which is the usual Class 2 case. **Outbound** is handover and upgrade: how a recipient takes the system, runs it, and stays able to receive your fixes, which can apply to a project of either class. A greenfield project that hands its system to a customer, a client, or an internal team has real Layer 6 content. Stub the inbound half when there is nothing to migrate from. Do not stub the layer.

### Document Dependency Order

The 14 normative documents produced by Stratum 2 are produced in dependency order — the document that feeds another is produced first. The Classification Record is the root; every document traces back to it.

A lower-layer document that contradicts a higher one is not a standalone error. It is a signal that the higher-layer document is incomplete or incorrect. The correct response is to return to the higher layer, revise it, and re-derive the lower one — not to resolve the contradiction locally in the lower document.

Documents are not finalized by completion. They are finalized by surviving challenge.

Each derivation gets its own numbered folder under `docs/strata/2.derivation/`, not a single file, because at real scale a derivation is several files. Name each file inside its folder for its content:

```
docs/strata/2.derivation/
├── 01-vision/                       project manifesto (optional) + vision & business case
├── 02-current-system/               Class 2 only
├── 03-brd/                          business requirements, BR-NNN
├── 04-prd/                          product requirements, PRD-NNN, and Feature Releases, FR-NNN
├── 05-functional-spec/              may be absorbed into 04
├── 06-nfr/
├── 07-target-architecture/          with its ADRs
├── 08-api-data-contracts/
├── 09-transition-strategy/          Layer 6, both directions
├── 10-risk-dependency-analysis/
├── 11-engineering-standards/        engineering manifesto (optional) + standards
├── 12-testing-strategy/
├── 13-devops-deployment-plan/
└── 14-master-implementation-plan/   optional
```

Two documents are conditional. Document 05, the Functional Specification, may be absorbed into document 04, because per-feature flows, states, and edge cases belong next to the acceptance criteria that test them. Document 14, the Master Implementation Plan, is optional on a project whose delivery sequence is already fixed upstream; where it exists, it sequences milestones rather than individual releases. The tree also holds two manifestos, and they sit at different nodes of the [authority chain](stratum-3-authority-chain.md): the project manifesto beside the vision states what gets built, and the engineering manifesto at the root of the standards states how it gets built.

### Requirements in Three Tiers

A requirement's identifier names the document it lives in. Business requirements (`BR-NNN`) live in document 03 and state what the business needs. Product requirements (`PRD-NNN`) live in document 04 and state what the system must do, each one a single statement that one test can pass or fail. A single statement is not something a human can demonstrate and sign off, so document 04 also bundles product requirements into the units that get gated:

| Tier | Test |
|---|---|
| `PRD-NNN` | A single verifiable statement. One test can pass or fail it |
| **`FR-NNN` — Feature Release** | The smallest set of `PRD-NNN` that can be deployed and demonstrated together in one execution cycle |
| **Milestone** | The smallest set of Feature Releases that reaches a state the human engineer would call a working increment |

The [Feature Release](https://strataprotocol.org/documentation/reference#feature-release) is the unit the Execution Loop runs against. It is named for the outcome it delivers rather than the mechanism it uses: `FR-002-document-ingestion-and-search`, never `FR-002-lancedb-integration`.

Document 04 carries scope, acceptance criteria, dependencies, and constraints. It does not carry the analysis or the phased implementation plan. Those are written in Stratum 4, when the Feature Release's session opens, with the accumulated context of every prior session. A plan derived months earlier is a plan written without the deviations the Execution Loop exists to feed forward.

### Generate → Iterate → Finalize

Every Stratum 2 document passes through three stages before it is normative:

**Generate** — Produce a first draft from available context. The draft makes thinking visible so it can be challenged. It is a starting point, not a deliverable.

**Iterate** — Challenge the draft against the documents above it in the dependency chain. Does this document contradict the Vision? Does this requirement trace to a business objective? Does this architecture decision have an ADR that justifies it? Unresolved contradictions are signals that a higher-layer document needs revision — not judgment calls to be made locally.

**Finalize** — When the document has survived challenge, declare it normative. Commit it to the repository and version it. A finalized document that must change requires a documented reason and a version increment.

> [!TIP]
> **Key insight:** The Derivation Agent generates and iterates with speed. Only you finalize. Finalization is a judgment call about whether a document has survived challenge well enough to be treated as normative — and that judgment belongs to the human engineer. The moment you treat AI-generated output as finalized without challenge, you have introduced an ungoverned assumption into your authority chain.

### Classification Sensitivity

Not all layers produce equally weighted documents for every project type. Each layer has a default depth keyed on the class, and the Classification Record from Stratum 1 carries a depth directive that overrides the default per layer, with a stated reason.

A **Class 1 Greenfield MVP** stubs the inbound half of Layer 6 (Transition) — not because migration is unimportant in principle, but because there is nothing to migrate from. A **Class 2 Established System** treats Layer 6 as a first-class risk surface. Its architecture decisions are business continuity decisions; its transition strategy requires the same rigor as its architecture document. The class default is only a default: a Category 1.3 platform with no continuity obligations can still need full Layer 4 and Layer 5 depth, because its constraint is irreversibility. Apply the record's directive, not the class default, whenever the two disagree.

## How Practitioners Apply It

### Begin with the Classification Record

Load the signed Classification Record from Stratum 1. This is the highest-authority document in the derivation chain. Every subsequent document traces to it — its depth directive determines which layers are full, lightweight, or stubbed.

### Derive Layer 1 — Vision & Business Case

Produce the Vision & Business Case first. It is the north star for every document that follows. Challenge it: Does it honestly state why the initiative exists? Does it commit to measurable success criteria? Does it define scope boundaries explicitly? A vision document that cannot be challenged is a vision document that will fail to govern.

### Work the layers in dependency order

Produce Layer 2 documents (Requirements) from Layer 1. Produce Layer 3 documents (Functional Specification) from Layer 2. Never produce a lower-layer document before the documents it depends on are finalized. Each time a lower-layer document surfaces a contradiction with a higher one, return to the higher layer — do not paper over the contradiction locally.

### Use the Derivation Agent with discipline

The Derivation Agent is a governed AI persona operating under the triple-lens model (Product Philosopher, Transformation Architect, Principal Engineer). Use it to draft — then challenge what it produces. The Agent derives; it does not finalize. Finalization is a human act.

### Commit and version each finalized document

Finalized documents are committed to the repository with a version reference. A finalized document that must change requires a documented reason and a version increment. Downstream documents that depend on the changed document are flagged for review.

## What Goes Wrong Without It

The most common failure mode: producing documents at the wrong layer.

Architecture decisions appear in business requirements — the BRD describes how the system will be built instead of what the business needs. Implementation details appear in the vision statement — the initiative is framed in technical terms before a business objective has been stated. Requirements don't trace to business objectives — features are built because they seemed useful, not because they address a stated business need.

The deeper failure: the document set is internally inconsistent, and no one knows it until the AI sessions begin generating against it. Each session produces output that is coherent within its own context window but architecturally inconsistent with what previous sessions produced. The contradictions compound silently — visible only in the bugs that surface during integration, never traceable to their root cause in an undisciplined derivation process.

## StrataProtocol.org Example

This platform was built under STRATA Protocol v1.0, which numbered requirements `REQ-NNN`, and its records keep those identifiers; FR-001 is its first [Feature Release](https://strataprotocol.org/documentation/reference#feature-release) under STRATA Protocol v1.2.

The `docs/strata/2.derivation/` directory is the complete Stratum 2 output for this platform — 14 normative documents produced in dependency order.

The derivation chain for this platform is intact and navigable: the Vision & Business Case established the north star; the Business Requirements (BRD) translated it into numbered business requirements; the Product Requirements (PRD) translated those into numbered product requirements, each with its own phased implementation plan; the Engineering Standards and Testing Standards defined how code would be written and verified.

The PRD that governs this very page's content (`REQ-001 — Public Framework Documentation`) was derived from the BRD, which was derived from the Vision, which was derived from the Classification Record. The dependency chain is traceable in both directions.

**Go deeper with a member account**

The Stratum 2 deep-dive covers the complete Derivation Engine: the Derivation Agent system prompt, worked examples for each of the 14 normative documents, challenge protocols for each layer, and document templates.

- [Access with a member account](https://strataprotocol.org/documentation/deep-dives/)

## Related Strata

**← Previous:** [Stratum 1 — Classification](stratum-1-classification.md) — provides the Classification Record that is Stratum 2's first and highest-authority input. The record's depth directive determines document depth, layer weighting, and which documents are required versus stubbed.

**→ Next:** [Stratum 3 — Authority Chain](stratum-3-authority-chain.md) — consumes the finalized Stratum 2 documents to construct the five-node authority chain. The operative constitution, issued as one edition per AI agent, aggregates essential constraints from the derivation document set into each agent's mandate.
