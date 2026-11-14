# Nuxt Development — Prompt Pack (Overview)

> **Public / redacted illustration.** This describes the *shape* of the STRATA prompt pack for a Nuxt
> Feature Release: its sections, and what each one takes in and hands on. It is intentionally **not** the
> prompts themselves — the working pack (exact wording, the parameters file, and the stack's checks) is
> part of the member toolkit.

A Feature Release is never delivered with a single clever prompt. It is delivered by driving an AI
copilot through a fixed, repeatable **sequence** — each prompt with a defined input, a defined output, and
a human checkpoint — so the work stays governed from rough idea to merged code. The value is in the
sequence, not in any one instruction.

## Driven by a parameters file

The pack is two files. The prompt pack holds the prompts, and its body is the same for the backend, frontend, and Nuxt
stacks. A companion parameters file holds every value that belongs to one project and one stack, and every prompt
opens by pointing to it. To start a release, both files are copied into the release's session folder and
the parameters are filled in. Prompts are then pasted in order as the work advances, and each one
stands alone, so any session can pick up the work.

## The sections

**Genesis · requirements document.** Rough specifications in, the release's requirements document out.
The agent consults the project's operative constitution first, and states each requirement so it can be
tested downstream.

**Preparatory documentation · analysis and implementation plan.** The requirements document in; the
**analysis**, the **phased implementation plan**, and the session index out. They are written when the
release's session opens, against the codebase as it stands and everything earlier releases learned, and
every phase carries its own acceptance criteria.

**Implementation.** Three prompts repeat for every phase:

- **Phase N · Kickoff** starts the phase from the plan and the records of the phases before it, and
  keeps rough, ephemeral working notes.
- **Phase N · Iteration K** folds in the findings of the human's manual testing. It repeats until
  testing passes.
- **Phase N · Close-out** runs only after the human gate passes. It distills the notes into a per-phase
  manifest and a running commit-log entry, and reconciles the plan for the phases still to come.

Every implementation prompt ends with the same protocol: keep the working notes, and clear the
verification gate before handing back.

## What the Nuxt parameters add

The prompts are the same as the backend and frontend packs. The Nuxt parameters file makes two
differences, and both come from where rendering, server, and access control meet:

- **Security-gated.** When a phase introduces protected content, the phase cannot close until access
  control is enforced at every required layer and its tests pass. This invariant outranks convenience.
- **Cost-aware.** Fast checks (type-checking and the test suite) run on every iteration, while the
  expensive full production build is left to the human engineer at the gate.

## Why it is shaped this way

The craftsmanship lives in a few deliberate design choices:

- **Human as the gate.** No phase advances, and no record is finalized, without explicit human
  confirmation.
- **Ephemeral notes, distilled records.** Rough notes capture the messy reality of a phase as it
  happens; the permanent manifest is written once, afterward — so the record is accurate, not aspirational.
- **Carry-forward context.** Each phase inherits the records of the phases before it, so a long release
  stays coherent and the resulting artifact trail is complete and correctly ordered.
- **Traceability by default.** Scope is declared up front and every deviation is logged, so each change
  is auditable back to the requirement that motivated it.

> Tuned for full-stack Nuxt work, where rendering, server, and access control meet. The backend and
> frontend packs share the same prompts without the SSR-specific security and build governance. The exact
> prompts are member-only.
