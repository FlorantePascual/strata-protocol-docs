# `docs/strata/5.artifact-trail` — Artifact Trail Index

**Stratum:** 5 — Artifact Trail

The artifact trail is the fifth and final stratum: the durable, navigable record of every governing decision a STRATA-driven project makes, from classification through delivery. It is not a separate set of documents — it *is* the accumulated output of strata 1–4, kept in one place and indexed so the chain of intent stays legible for the life of the project.

This index is the public, redacted artifact trail for **StrataProtocol.org** — the platform that hosts the STRATA Protocol, governed by the protocol itself. Every link below points to a real governance document in this repository under [`docs/strata/`](../). A stratum with a single document links straight to the file; a stratum with several links to its folder. Some Stratum 2 slots and the later execution-loop sessions are held back by design — enough is shown to substantiate the methodology without exposing proprietary detail.

The populated records date from STRATA Protocol v1.0, when requirements were numbered `REQ-NNN`. From STRATA Protocol v1.1 on, requirements are `PRD-NNN` and the execution loop runs one Feature Release, `FR-NNN`, at a time. This platform's first Feature Release, FR-001, is present as a placeholder. The [folder README](../README.md) explains the template, its placeholders, and the expected tree at STRATA Protocol v1.2.

---

## Stratum 1 — Classification

- [Classification Record](../1.classification/classification-record.md) — the signed record fixing this project as **Class 1, Category 1.3 — Platformization**, before any specification was written. Classification sets the engineering posture everything downstream inherits.

## Stratum 2 — Derivation

- [Vision & Business Case](../2.derivation/01-vision/vision-business-case.md) — Layer 1 (*Why*): the problem, the opportunity, and the business objectives.
- [Business Requirements](../2.derivation/03-brd/business-requirements.md) — Layer 2 (*What*): the business requirements derived from the vision.
- [Product Requirements](../2.derivation/04-prd/) — Layer 2 (*What*): the product requirements derived from the business requirements, and the release records that group them for delivery. The `REQ-NNN` records are this platform's v1.0-era requirements; the FR-001 placeholder marks its first Feature Release.
- [Engineering Standards](../2.derivation/10-engineering-standards/) — Layer 5 (*Quality*): the content, testing, and UI standards every page and module is held to.

> Some Stratum 2 slots (current-system assessment, functional specs, NFRs, target architecture, API/data contracts, migration strategy) carry `.gitkeep` placeholders in this public copy. They exist to show the complete derivation structure a governed project maintains, even where the artifact itself is not published here.

## Stratum 3 — Authority Chain

- [Authority Chain](../3.authority-chain/) — the operative constitution that governs every AI coding session and human contribution on this codebase. It is one constitution issued as one edition per AI agent; this folder holds high-level public summaries of the Claude Code and GitHub Copilot editions. It binds the agent to the active Feature Release and its phase plan, and encodes the project's non-negotiable architectural and security rules.

## Stratum 4 — Execution Loop

- [Execution-Loop Sessions](../4.execution-loop/) — a sample of governed delivery sessions (analysis, implementation plan, phase notes, per-phase manifests), one session folder per release. One representative session is shown end-to-end; later loops are redacted to protect proprietary detail, and FR-001's session is present as a placeholder index.

## Stratum 5 — Artifact Trail

The artifact trail is the whole collection — the record above, kept and cross-linked across the project's lifecycle. Browse the complete [`docs/strata` tree](../), or start from the [strata README](../README.md).

---

For the reusable delivery scaffolding behind these documents — the prompt packs and templates that drive each execution loop — see [`docs/_templates/`](../../_templates/). The full implementation templates, AI prompt library, and developer guide are available to members at [strataprotocol.org](https://strataprotocol.org).
