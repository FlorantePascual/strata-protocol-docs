# STRATA

**Structured, Traceable, Repeatable Automation — with Technical Authority**

> _STRATA governs AI-assisted software delivery through layered authority, derivation, and human-gated execution._

Published by **[StrataProtocol.org](https://strataprotocol.org/)**

---

## What STRATA is

STRATA is a software delivery methodology built for the era of AI-assisted development. It does not treat the AI copilot as a productivity shortcut — it treats it as a governed actor operating inside a defined mandate. Every session, every phase, every output is anchored to an authority chain that the human engineer controls and the AI cannot override.

The name reflects the methodology's structure: five load-bearing strata, each one foundational to everything above it. Remove any stratum and the system above it loses integrity.

STRATA is not a project management framework. It is not agile, waterfall, or a hybrid of either. It is a **delivery governance system** — the layer that sits above your tooling, your stack, and your process and makes all of them coherent.

---

## The five strata

### Stratum 1 — Classification

_Pre-development intelligence. Landscape, scope, and limits defined before any spec is written._

Every software initiative begins by answering a single question: **what kind of system is this?** The answer determines everything downstream — documentation depth, risk tolerance, AI session constraints, and acceptable scope of change.

STRATA recognizes two primary project types, each with sub-categories:

**Startup / MVP system**

- Greenfield with validation intent
- Prototype with production potential
- Speed-primary, change-tolerant, low continuity dependency

**Established system**

- Continuity-critical (live customers, contractual obligations)
- Operationally sensitive (uptime, data integrity, compliance exposure)
- Mature codebase with accumulated technical decisions
- Customer-dependent (breaking changes have downstream cost)

Classification is not a formality. It is the first act of technical authority. A misclassified project will produce specs written at the wrong depth, architecture decisions made at the wrong risk tolerance, and AI sessions operating without the right constraints.

**Output of Stratum 1:** A signed classification record — project type, sub-category, and the continuity/risk/maturity rationale that justifies the classification.

---

### Stratum 2 — Derivation

_Business intent cascades into technical artifacts. Documents are derived, not written._

Each document in STRATA answers exactly one framing question. If you cannot state the question, you are not ready to produce the document.

|Framing question|Document produced|
|---|---|
|Why are we doing this?|Vision and business case|
|What does the business need?|Business requirements (BRD)|
|What should the system do?|Product requirements (PRD)|
|How will users interact with it?|Functional specification, or the same content inside the PRD|
|What quality must it hold to?|Non-functional requirements|
|How will it be built?|Architecture and technical design|
|What decisions did we make and why?|Architecture Decision Records (ADRs)|
|What open questions need resolution?|Requests for Comment (RFCs)|
|What is the delivery plan?|Master implementation plan, optional|

Documents are produced in dependency order — no architecture document is written before requirements are stable, no delivery plan is written before architecture is settled. Each document is challenged and refined before the next is derived from it.

Requirements descend through three tiers, and only the last one is gated:

|Tier|What it is|
|---|---|
|`BR-NNN`|A business requirement. What the business needs|
|`PRD-NNN`|A product requirement. One verifiable statement about system behavior|
|`FR-NNN-[narration]`|A **Feature Release**. The smallest set of product requirements that can be deployed and demonstrated together, and the unit the execution loop runs against|

A Feature Release carries a unique identifier that threads through every subsequent artifact, commit and session. Its preparatory analysis and its phased implementation plan are written in Stratum 4, when its session opens, against the accumulated context of every prior session.

**Output of Stratum 2:** A complete, challenged, and refined document set. When this stratum is complete, the _why_, _what_, _how_, and _when_ of the system are unambiguous.

---

### Stratum 3 — Authority chain

_Every AI session is constitutionally bound. The copilot is a governed actor, not a free agent._

Before any copilot session begins, an authority chain is established. The chain is a hierarchy of documents, each one constraining everything below it. The **operative constitution** sits at the bottom, derived from everything above it.

```
Project manifesto
  → Vision and business case
    → Requirements
      → Standards
        → operative constitution   ←  AI operative constitution
             one node, issued as one edition per agent
             mastered in 3.authority-chain/, deployed to each agent's path
```

Every node names a source document, because a chain of descriptions cannot be assembled without guessing.

|Node|Source|
|---|---|
|Project manifesto|`2.derivation/01-vision/project-manifesto.md`, written by the project author|
|Vision and business case|`2.derivation/01-vision/vision-business-case.md`|
|Requirements|`2.derivation/03-brd/` and `04-prd/`|
|Standards|`2.derivation/11-engineering-standards/`, rooted at the engineering manifesto, plus `12-testing-strategy/`|
|Operative constitution|`3.authority-chain/`, holding one master per edition, each deployed to its agent's path|

**Two manifestos exist and they sit at different nodes.** The project manifesto constrains what gets built. The engineering manifesto, written by the team, constrains how it gets built. Both are optional and both are recommended.

**One constitution, several editions.** `.github/copilot-instructions.md` is one place node 5 can live. `CLAUDE.md`, `.cursor/rules/` and `AGENTS.md` are others, and most projects run more than one agent. These are not one text copied to several paths: Cursor reads a directory of rule files with frontmatter globs, Copilot reads one workspace file, `CLAUDE.md` is read by humans too. Node 5 is therefore **one constitution issued as one edition per agent**, each in that agent's native form. The masters sit under `3.authority-chain/` in a tree mirroring the deployed paths, and each is cut to its live path as an exact copy. Two invariants hold them together: **fidelity** between a master and its deployed copy, and **parity** between editions, which is checked mechanically rather than asserted in a table. The constitution set is declared on the classification record. Stratum 3 carries the rules.

**What the authority chain governs:**

- What the AI is permitted to generate
- What conventions, patterns, and standards it must follow
- What it must not do without explicit human instruction
- How it must structure its outputs for human review

The authority chain is not a prompt. It is a governance document. A prompt asks the AI for something. The authority chain defines the conditions under which the AI operates — permanently, for the duration of the project.

Each new project generates a fresh authority chain, derived from that project's classification and document set. The chain is versioned. Where the derivation output lives outside the code repository, the deployed editions carry the constitution version they were cut from, because nothing else in the repo can establish whether they are current.

**Output of Stratum 3:** A complete authority chain culminating in a project-specific operative constitution, issued as one edition per agent and deployed to every path those agents load as their operating context for every session.

---

### Stratum 4 — Execution loop

_Phased, per-release development. No phase advances without human sign-off._

STRATA's execution model is a loop, not a pipeline. Each Feature Release moves through its phases independently. Each phase is a governed cycle, not a single call.

```
┌─────────────────────────────────────────────────────┐
│                  Execution loop                     │
│                                                     │
│  Copilot generates    →   Human reviews             │
│  (full context,           manually tests,           │
│   authority chain)        signs off                 │
│         ↑                      │                    │
│         │              Deviations, fixes,           │
│         └──────────    unplanned scope              │
│                        fed forward as               │
│                        next-phase context           │
│                                                     │
│                   Phase gate                        │
│              approved → push to repo                │
│              → next phase begins                    │
└─────────────────────────────────────────────────────┘
```

**How each phase works:**

1. The copilot receives the phase plan, the full authority chain, and the accumulated context from all prior phases
2. The copilot generates code in a structured, prompt-driven workflow — not in a single call
3. The human engineer reviews the output, manually tests the build, and either approves or returns with corrections
4. If corrections are needed, the loop runs again within the same phase until the output meets the standard
5. On approval, the phase is signed off, changes are pushed to the repository, and context is updated
6. Any deviations from the original plan, iteration fixes, or unplanned scope are explicitly documented and injected into the next phase's context
7. The next phase begins with full awareness of everything that actually happened — not just what was planned

The human engineer is not a reviewer at the end of the process. The human is the **gate** — the mechanism that makes the loop trustworthy. No phase advances on the copilot's judgment alone.

**Output of Stratum 4:** Approved, committed code per phase, per Feature Release — with a complete record of what was planned, what was built, what deviated, and why.

---

### Stratum 5 — Artifact trail

_Every decision, deviation, and sign-off is documented and fed forward._

STRATA treats documentation as a first-class output, not an afterthought. The artifact trail is the living record of the system as it was actually built — not as it was originally planned.

**Artifact types:**

|Artifact|Purpose|
|---|---|
|ADR log|Every architecture decision with its context, options considered, and rationale|
|Phase sign-off records|What was approved, when, and by whom|
|Deviation log|What changed from the plan, why, and how it was handled|
|Git commit narratives|Human-readable, generated alongside each commit — not shorthand|
|Context forward notes|Structured summaries injected into the next phase's copilot context|
|Session anchors|The authority chain state at the start of each session|

The artifact trail is not documentation for its own sake. It is the mechanism by which future development — whether by the same engineer, a new team member, or a future copilot session — inherits the full truth of how the system was built. A codebase without an artifact trail is a system with amnesia.

**Output of Stratum 5:** A complete, living project record that persists beyond any individual development session and makes every future decision an informed one.

---

## The governing principles

**Classify before you specify.** The project type determines everything downstream. A misclassification is the most expensive mistake STRATA can encounter — it cannot be corrected cheaply after documents are written.

**Derive, don't write.** Every document answers a framing question. Writing a document without first stating the question it answers is producing output without purpose.

**The AI is governed, not autonomous.** The authority chain defines what the copilot may do and how it must think. It operates inside a mandate. A copilot without a mandate is not a tool — it is a liability.

**The human is the gate, not the reviewer.** No phase advances without explicit sign-off. The gate is not a quality checkpoint at the end of the process — it is the mechanism that makes the loop trustworthy at every step.

**Deviations are context, not failures.** Every unplanned change, iteration fix, or scope discovery is fed forward as explicit context. Future phases inherit the full truth of what happened — not a sanitized version of the plan.

**Repeatability is the product.** STRATA and the platform-first architecture it governs are both designed to be reused across engagements. The goal is not to deliver one system well — it is to deliver every system well, faster than the last.

**The artifact trail outlives the project.** Documentation, commit narratives, and decision logs are first-class outputs. A system whose construction cannot be reconstructed from its records is a system that will be misunderstood by everyone who touches it next.

---

## How STRATA relates to Platform-First delivery

STRATA is the _process_. Platform-First is the _architecture_.

The Platform-First delivery model — a battle-tested, shared NestJS backend core from which thin domain-specific services are derived and independently deployed — is governed end-to-end by STRATA. Classification determines whether a new engagement extends the existing platform or requires a new derived service. Derivation produces the domain-specific architecture and endpoint specs. The authority chain encodes the platform's conventions into the copilot's operating context. The execution loop builds only what is genuinely new. The artifact trail records every extension to the platform for future reuse.

Together they form a complete practice:

```
StrataProtocol.org
  ├── STRATA Protocol          (this document — delivery governance)
  └── Platform-First model     (shared backend core — technical architecture)
```

---

## Extension vocabulary

|Term|Definition|
|---|---|
|**STRATA Protocol**|This document — the canonical practitioner reference|
|**STRATA Delivery Model**|Client-facing framing for how engagements are scoped and run|
|**STRATA Classification Engine**|Stratum 1 as a standalone tool for project typing|
|**STRATA Authority Chain**|The AI governance layer — constitution, session anchoring, copilot mandate|
|**STRATA Loop**|The execution model — phase-gated, human-validated, deviation-aware|

---

## Quick reference

```
STRATA
├── 01 Classification     pre-dev intelligence · project typing · risk framing
├── 02 Derivation         why → what → how → build · documents derived not written
├── 03 Authority chain    manifesto → requirements → copilot constitution
├── 04 Execution loop     generate → review → gate → push → repeat
└── 05 Artifact trail     ADRs · deviation log · commit narratives · context forward
```

---

## Version history

**Current: v1.2.** The full history, with what changed at each version and what a
project derived against an earlier one has to do about it, is in
[`CHANGELOG.md`](CHANGELOG.md). It is the one home for that record; this file
does not restate it.

| Version | Absorbs |
|---|---|
| **v1.2** | Seven corrections. Six finish the `REQ-NNN` retirement v1.1 started. The seventh replaces four named build commands with the four properties they were standing in for, and is the only change in v1.2 that affects conformance |
| v1.1 | 33 findings from the first full application of the protocol to a production project, a Class 1 category 1.3 platformization build delivered as a monorepo of eight applications across three ownership layers |

The findings registers that produced each version are kept at
[`change-history/`](change-history/), one file per pass, with the reasoning
behind every change.

---

_[StrataProtocol.org](https://strataprotocol.org/) · STRATA Protocol v1.2_