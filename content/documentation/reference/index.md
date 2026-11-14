---
title: "Glossary — Key Terms"
description: "Foundational STRATA Protocol terms for new practitioners."
navigation:
  order: 0
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-25"
---

This glossary defines the foundational vocabulary of the STRATA Protocol framework. Twelve key terms are presented here — enough to understand the framework's governance model and navigate its documentation with confidence. The complete STRATA vocabulary, covering 60 terms across all five strata, is available in the full member glossary via the link at the bottom of this page.

## Terms

### A

#### Artifact trail

The complete record of all governance artifacts produced during a project, maintained in `docs/strata/`. It includes every normative document — from the classification record through to execution loop session folders — providing end-to-end traceability across all governance decisions.

*Used in:* Stratum 5 — Artifact Trail; every STRATA project requires a populated artifact trail before implementation work begins.

#### Authority chain

The five-node chain of documents that governs what an AI copilot may do and how it must reason on a given project. The nodes run Project Manifesto → Vision & Business Case → Requirements → Standards → Operative constitution, and authority flows downhill: each node derives from the nodes above it and constrains the nodes below. Node 5, the operative constitution, gathers the upstream mandate into the form an AI agent loads at session start. It is one constitution issued as one edition per agent in use, each written in that agent's native format, and every edition imposes the same mandate.

*Used in:* Stratum 3 — Authority Chain; every governed session is anchored to the chain's current state, and the agent loads its edition of the operative constitution before generating any output.

### C

#### Classification

Stratum 1 of the STRATA Protocol — the act of categorizing a project by class and category before any specification work begins. A project's classification determines which derivation paths are appropriate and constrains every downstream decision.

*Used in:* Applied first, before any derivation document is authored; the classification record produced in Stratum 1 is the root document of every project's derivation chain.

#### Conditioning

The process of transforming raw governance artifacts from `docs/strata/` into MDC-formatted, navigation-annotated content suitable for publication in `content/`. Conditioning preserves the source document's meaning while adapting its structure for Docus rendering and sidebar navigation.

*Used in:* Applied when internal strata documents are to be published as platform documentation; each conditioned page requires a frontmatter update and a pass to ensure MDC-compatible structure.

### D

#### Derivation

Stratum 2 of the STRATA Protocol — the process of producing governance documents layer by layer, each derived from the one above it. No document in the derivation chain is authored without a traceable source in the layer above.

*Used in:* Applied continuously throughout the specification phase; the derivation chain runs Vision → BRD → PRD, with each document answering the framing question of the stratum it belongs to.

### E

#### Execution loop

Stratum 4 of the STRATA Protocol — the human-gated, phase-by-phase delivery cycle through which governed AI and engineer build software together. Every phase ends at a mandatory phase gate; the AI copilot cannot advance without explicit human sign-off.

*Used in:* Applied during all development work; each feature's implementation is structured as a phase plan within a session folder in the execution loop.

### F

#### Feature Release

The unit the Execution Loop runs against: the smallest set of product requirements that can be deployed and demonstrated together in one execution cycle. A Feature Release is identified as `FR-NNN-[narration]`, named for the outcome it delivers rather than the mechanism it uses, and carries the acceptance sentence the human engineer validates at its gate.

*Used in:* Defined in Stratum 2 (Derivation) and executed in Stratum 4 (Execution Loop), where one session opens per Feature Release; it replaced the individual requirement as the gated unit in STRATA Protocol v1.1.

### G

#### Governed AI

An AI coding tool operating within the constraints of an established authority chain and execution loop. A governed AI's scope, reasoning, and output are constrained by a chain of human-approved documents — it does not determine its own scope or advance phases unilaterally.

*Used in:* Applied throughout Strata 3, 4, and 5; defines the operating relationship between the human engineer and the AI copilot on any STRATA-governed project.

### P

#### Phase gate

A defined checkpoint within the Execution Loop at which the human engineer must explicitly approve before the next phase begins. A phase gate is a structural governance control — it cannot be delegated to the AI copilot or skipped.

*Used in:* Applied at the boundary between every phase in Stratum 4; no phase may begin until the prior phase gate is explicitly cleared by the human engineer.

#### Protected route

A content path registered in `PROTECTED_ROUTES` that requires an authenticated session with the correct role before content is served. Frontmatter `requiredRole` alone does not protect content — the path must also appear in `PROTECTED_ROUTES` to block direct content API calls.

*Used in:* Applied in Stratum 4 and 5 when implementing member-only content; both the browser-side route guard and the server-side content guard consult `PROTECTED_ROUTES` independently.

### R

#### Role (RBAC)

A named access level — `public`, `customer`, `partner`, or `admin` — that controls which content a user can access on this platform. Roles are stored in the session and checked by both the browser route guard and the server content guard on every request.

*Used in:* Applied throughout the platform to gate member-only content and implementation templates; the role hierarchy is tier 0 (`public`) through tier 3 (`admin`).

### S

#### Stratum

One of the five load-bearing layers of the STRATA Protocol governance framework. Each stratum has one framing question and one output layer; the five strata are Classification, Derivation, Authority Chain, Execution Loop, and Artifact Trail.

*Used in:* Applied across the entire framework; practitioners refer to a stratum by number (e.g., "Stratum 2 — Derivation") when citing which governance layer a document or process belongs to.

---

::u-page-c-t-a
---
title: Access the complete STRATA vocabulary
description: The full glossary contains 60 terms covering all five strata, the release and constitution vocabulary, implementation templates, derivation processes, and comparative definitions — the complete reference for practitioners applying STRATA daily.
variant: soft
links:
  - label: Access the full glossary
    to: /documentation/reference/full-glossary
    icon: i-ph-lock
    color: neutral
---
::
