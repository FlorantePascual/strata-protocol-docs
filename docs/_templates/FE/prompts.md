# Frontend Development — Prompt Sequence (Overview)

> **Public / redacted illustration.** This describes *what* the STRATA prompt sequence for a frontend
> feature does, and *why* it is shaped this way. It is intentionally **not** the prompts themselves —
> the working prompt library (exact wording, orchestration, and per-phase scripting) is part of the
> member toolkit.

A feature is never delivered with a single clever prompt. It is delivered by driving an AI copilot
through a fixed, repeatable **sequence** — each stage with a defined input, a defined output, and a
human checkpoint — so the work stays governed from rough idea to merged code. The value is in the
sequence, not in any one instruction.

## Stage 1 — Planning

The opening prompt turns an informal, rough specification into a rigorous requirements document. The
agent is directed to reconcile the request against the existing system rather than accept it at face
value, and to make a deliberate second pass to close gaps before anything is designed. The output is a
single authoritative requirement the rest of the work derives from.

## Stage 2 — Documentation

From the approved requirement, the agent produces two artifacts: an **analysis** (structure, risks,
impact, open questions) and a **phased implementation plan** with its own acceptance criteria per phase.
Reference templates exist, but the agent is told to reason against the *current* codebase rather than
copy them — boilerplate is treated as a suggestion, never an answer.

## Stage 3 — Implementation

Implementation runs as a human-gated loop, one phase at a time:

- **Iterate.** The agent implements a phase while keeping rough, ephemeral working notes. Each
  subsequent iteration folds in the human's review feedback and continues until manual testing passes.
- **Verify.** Any change to code logic must clear a verification gate — type-checking, build, and the
  test suite — before the agent is allowed to yield.
- **Document, only after sign-off.** No durable documentation is written until the human confirms the
  phase actually works. Then the agent distills its working notes into a permanent record: a per-phase
  manifest of everything done, an entry in a running commit log, and a structured, ready-to-use commit
  message.

## Why it is shaped this way

The craftsmanship lives in a few deliberate design choices:

- **Human as the gate.** No phase advances, and no record is finalized, without explicit human
  confirmation.
- **Ephemeral notes, distilled records.** Rough notes capture the messy reality of a phase as it
  happens; the permanent manifest is written once, afterward — so the record is accurate, not aspirational.
- **Carry-forward context.** Each phase inherits the records of the phases before it, so a long feature
  stays coherent and the resulting artifact trail is complete and correctly ordered.
- **Traceability by default.** Scope is declared up front and every deviation is logged, so each change
  is auditable back to the requirement that motivated it.

> Tuned for frontend work — components, state, rendering, and user-facing behavior. The backend and Nuxt
> sequences follow the same shape with stack-appropriate verification and governance. The exact prompts
> are member-only.
