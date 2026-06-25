# Nuxt Development — Prompt Sequence (Overview)

> **Public / redacted illustration.** This describes *what* the STRATA prompt sequence for a Nuxt
> feature does, and *why* it is shaped this way. It is intentionally **not** the prompts themselves —
> the working prompt library (exact wording, orchestration, and per-phase scripting) is part of the
> member toolkit.

A feature is never delivered with a single clever prompt. It is delivered by driving an AI copilot
through a fixed, repeatable **sequence** — each stage with a defined input, a defined output, and a
human checkpoint — so the work stays governed from rough idea to merged code. The Nuxt sequence follows
the same shape as the backend and frontend ones, with three deliberate differences noted below: it is
**constitution-aware**, **security-gated**, and **cost-aware**.

## Stage 1 — Planning

The opening prompt turns an informal, rough specification into a rigorous requirements document. Before
it writes anything, the agent consults the project's operative constitution — the Stratum 3 authority
chain — so the requirement is shaped *within* the project's hard constraints from the outset rather than
corrected against them later. A deliberate second pass closes gaps before anything is designed.

## Stage 2 — Documentation

From the approved requirement, the agent produces two artifacts: an **analysis** (structure, risks,
impact, open questions) and a **phased implementation plan** with its own acceptance criteria per phase.
The agent reasons against the *current* codebase rather than copying reference templates. Where a
feature touches access-controlled content, the planning artifacts must carry the project's security
checklist so the constraint is designed in, not bolted on.

## Stage 3 — Implementation

Implementation runs as a human-gated loop, one phase at a time:

- **Iterate.** The agent implements a phase while keeping rough, ephemeral working notes. Each
  subsequent iteration folds in the human's review feedback and continues until manual testing passes.
- **Guard the security boundary.** When a phase introduces protected content, the loop carries an
  additional checklist: access control must be enforced at every required layer before the phase can
  close. This invariant outranks convenience.
- **Verify — cost-aware.** Any change to code logic must clear a verification gate, but the gate is
  tuned for cost: fast checks (type-checking and the test suite) run every iteration, while the
  expensive full build is deferred to the human.
- **Document, only after sign-off.** No durable documentation is written until the human confirms the
  phase works. The agent then distills its notes into a per-phase manifest, a running commit-log entry,
  and a structured commit message — and, for protected content, records that the security checklist was
  satisfied.

## Why it is shaped this way

The craftsmanship lives in a few deliberate design choices:

- **Human as the gate.** No phase advances, and no record is finalized, without explicit human
  confirmation.
- **Security is an invariant, not a feature.** The access-control checklist travels with the work and
  must be satisfied before a phase is considered done.
- **Ephemeral notes, distilled records.** Rough notes capture the messy reality of a phase as it
  happens; the permanent manifest is written once, afterward — so the record is accurate, not aspirational.
- **Carry-forward context.** Each phase inherits the records of the phases before it, so a long feature
  stays coherent and the resulting artifact trail is complete and correctly ordered.

> Tuned for full-stack Nuxt work, where rendering, server, and access control meet. The backend and
> frontend sequences follow the same shape without the SSR-specific security and build governance. The
> exact prompts are member-only.
