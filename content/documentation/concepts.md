---
title: Core Concepts
description: The governing principles and mental models that run through all five strata.
navigation:
  order: 4
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-24"
---

The five governing principles below are not aspirational guidelines. They are the structural axioms that make the five-stratum architecture coherent. Strip any one of them and the strata no longer interlock: classification cannot be trusted without derivation discipline, derivation documents are ungoverned without an authority chain, the authority chain has no enforcement without the execution loop, and the execution loop produces nothing durable without the artifact trail. Every document produced under STRATA, every gate decision, every session protocol is defensible because these principles were applied — consistently, at every layer, without exception.

### Principle 1 — Derive, Don't Invent

**Statement**

Every document in a STRATA-governed project traces to a higher-layer intent. Nothing is created from first principles at the wrong layer of the authority chain. The Classification Record constrains the vision; the vision constrains the business requirements; the business requirements constrain the product specification; the product specification constrains the implementation plan. The arrow of causation runs downward: each layer derives from the layer above it. Documents that cannot state their derivation source are ungoverned decisions masquerading as governed work.

**Implication for practitioners**

Before authoring any document, answer one question: "What higher-layer document authorizes this?" If you cannot name a specific document, stop — you are about to invent rather than derive. The most common governance failure is not an organization that ignores process: it is one that believes it follows a framework because it has produced documents, while those documents were authored in parallel, independently, by practitioners who never asked what authorized them to exist.

---

### Principle 2 — Human as the Gate

**Statement**

No phase of work advances without explicit sign-off from a human decision-maker. AI-generated output — regardless of apparent quality or confidence — is never self-authorizing. The gate is not a review step appended to the end of a workflow; it is the structural boundary between work-in-progress and work-accepted. Gate criteria are defined before the sprint begins, not constructed after seeing the output.

**Implication for practitioners**

A gate defined after the output is seen is not a gate — it is an acceptance ceremony. When practitioners construct their criteria to match what was delivered rather than what was required, the human loses the only moment at which genuine judgment can be exercised. The practitioner who skips the explicit gate becomes accountable for decisions they did not actually make: they approved the output, not the reasoning behind it.

---

### Principle 3 — Governance by Layer

**Statement**

Each of the five strata has a single framing question. That question defines the stratum's scope exactly — and its boundary with adjacent strata equally exactly. Classification answers "What kind of system is this?" Derivation answers "What documents govern it?" Authority Chain answers "Who and what controls the AI?" Execution Loop answers "How is work authorized and executed?" Artifact Trail answers "What record exists of every decision?" Mixing concerns across strata creates the governance failures STRATA is designed to prevent.

**Implication for practitioners**

When a document contains concerns from multiple strata, stop and separate them. Each concern belongs in exactly one stratum. Implementation detail inside a Vision document collapses two layers into one — losing the strategic clarity a Vision provides and the technical precision an implementation plan requires. Layer discipline is the hardest STRATA habit to build: practitioners consistently feel they are saving time by combining concerns, and consistently find later that they have created the conditions for governance drift.

::tip
The five principles are mutually reinforcing. Derive, Don't Invent only works if a human holds the gate — otherwise AI output becomes the authorizing source. Governance by Layer only holds if the artifact trail records where each concern was assigned. Remove any one principle and the others degrade. Apply all five consistently and the governance structure becomes self-correcting.
::

---

### Principle 4 — Traceability by Design

**Statement**

Every significant decision in a STRATA-governed project is recorded in the artifact trail at the moment the decision is made — not reconstructed afterward. The artifact trail is not a retrospective summary: it is the live record of the project's decision-making history. Traceability is a property of the construction process, not a documentation layer applied on top of it after construction is complete.

**Implication for practitioners**

The discipline of traceability is a discipline of timing. Producing an ADR during the session where a decision is made costs minutes and preserves the full context — including the trade-offs considered, the constraints in play, and the options rejected. Producing it afterward costs hours and yields a document that describes what was decided, not why. The practitioner who defers artifact production until after the sprint produces a trail that proves the project was managed, but cannot explain how or why it was managed as it was.

---

### Principle 5 — Self-Referential Proof

**Statement**

The STRATA Protocol's primary evidence of validity is StrataProtocol.org itself. The platform was designed, derived, governed, executed, and documented using STRATA — and the artifact trail that proves this is publicly accessible in the project's `docs/strata/` hierarchy. A governance framework that cannot be applied to its own creation is a framework that exists only in theory. The self-referential proof is the mechanism by which STRATA avoids that failure: every claim about the framework can be verified against the project that built it.

**Implication for practitioners**

For any organization adopting STRATA, the first governed project is simultaneously a training exercise and a proof of concept. Practitioners who document their adoption — as StrataProtocol.org documents its own construction — create an artifact trail that makes every subsequent adoption cheaper, more confident, and better governed. The self-referential commitment transforms the framework from a methodology that claims to work into one that can be inspected and verified.

::u-page-c-t-a
---
title: Go deeper with a member account
description: The member-only deep-dives translate each governing principle into procedural templates — operative constitution formats, phase gate checklists, ADR schemas, and derivation worksheets that practitioners can apply immediately.
variant: soft
links:
  - label: Access with a member account
    to: /documentation/deep-dives/
    icon: i-ph-lock
    color: neutral
---
::
