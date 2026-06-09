---
title: "Stratum 4 — Execution Loop"
description: Phased, per-requirement development with human-gated phase advancement.
navigation:
  order: 4
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-24"
---

> *The human is the gate, not the reviewer. No phase advances without explicit sign-off. The loop is the mechanism that makes AI-assisted development trustworthy.*

Stratum 4 — Execution Loop is where STRATA becomes operational. Strata 1 through 3 are preparation — classify, derive, govern. Stratum 4 is execution: the structured, repeating cycle through which a governed AI copilot and a human engineer build software together, one phase at a time, one requirement at a time, against the defined mandate established by the authority chain.

The Execution Loop is not a development methodology bolted onto an AI tool. It is a purpose-built protocol for human-AI collaborative development, designed around one governing reality: AI-generated code cannot be trusted without a human gate, and a human gate without structure is not a gate — it is a formality.

::strata-diagram{name="strata_loop" alt="STRATA Execution Loop — four-stage clockwise cycle of Generate, Review, Gate, and Push with a correction arc back to Generate"}
::

## Why This Stratum Exists

Without structure, AI-assisted development degrades into five recognizable failure modes — each with a name in the STRATA Protocol:

- **Context vacuum**: A new session opens without context from prior phases. The copilot generates against its training defaults and the immediate conversational context — not against the authority chain, the current phase plan, or the deviations from prior phases.
- **Silent assumption**: The copilot encounters an ambiguity and resolves it with a reasonable-seeming assumption rather than surfacing it. The assumption surfaces as a bug two phases later.
- **Scope creep sprint**: During generation, the copilot adds something related and useful. The gate was defined for the original scope. The addition is unverified.
- **Rubber-stamp gate**: Automated checks pass. The code looks reasonable. The human marks the gate PASS without manually testing the feature. The gate becomes a formality.
- **Lost deviation**: A phase encounters an architectural finding, handles it, and moves on. No one documents it. A later phase introduces code that conflicts with the undocumented decision.

The Execution Loop provides a specific protocol mechanism to prevent each of these failure modes.

## Key Concepts

### Phase Structure

A phase is the smallest independently deliverable unit of a requirement. It has four properties:

1. It can be built by the copilot in a bounded sprint session
2. It can be manually tested by the human engineer against specific gate criteria
3. It produces a coherent, committable state of the codebase
4. It serves as a reliable foundation for subsequent phases

A phase that cannot be described in a single clear scope statement is too large. A phase that produces no independently testable output is too small. The phase definition — written before the sprint begins — is the copilot's primary work instruction.

### Human Gate

The gate is the human engineer's structured verification of a completed phase before it advances. It is not a code review. It is a **certification** that the phase's deliverables meet their criteria, the build is verified, and the feature behaves correctly under manual testing.

The gate has three possible outcomes:

- **PASS**: All gate criteria met. No unresolved issues. Proceed to artifact generation and next phase.
- **RETURN**: One or more gate criteria failed, or testing revealed an issue. Specific failures are provided to the copilot — not "it doesn't work" but the exact criterion that failed, the observed behavior, the expected behavior, and the files to correct. The copilot executes a fix iteration. Gate review resumes when all automated checks are clean again.
- **HOLD**: The phase passes its gate criteria, but testing revealed a scope question or architectural implication that must be resolved before the next phase begins. Artifacts are generated; the next phase does not begin until the hold is cleared.

Manual verification cannot be delegated to automated tests alone. Automated tests verify code correctness. The gate verifies feature correctness — that the feature behaves correctly as a user would experience it.

::note
Gate criteria defined before the sprint are the only criteria that count. Criteria written after seeing the output are criteria shaped to match the output — which converts the gate from a certification into a formality. If you cannot write specific, testable gate criteria before the sprint begins, the phase scope is not well-enough defined to execute. Return to the phase plan and tighten the scope before opening the session.
::

### Session Protocol vs. Gate Protocol

**Session protocol** governs how a phase is executed:

1. **Context loading**: The human provides the session context — requirement identity, phase number, plan reference, and context carried forward from all prior phases. The copilot confirms understanding before generation begins.
2. **Sprint execution**: All phase files generated in one sprint. Deviations logged in `phase-n-notes.txt` as they occur, not retrospectively.
3. **Automated quality verification**: `npm run type-check`, `npm run lint`, `npm run test:unit -- --run`, `npm run build` — all four must pass before the manual gate opens. Any failure is resolved by the copilot in the same session.
4. **`phase-n-notes.txt`**: The living in-flight record of the phase. Written during the initial sprint, appended on every fix iteration. Never overwritten — only appended. The honest account of what happened, including deviations.

**Gate protocol** governs what the human verifies and what artifacts are generated on sign-off. Gate criteria are defined before the sprint — specific and testable, not vague ("the node renders with the correct dashed border when isFractalParent is true" — not "the feature works").

::tip
The `phase-n-notes.txt` is the most important artifact produced during a phase. It is the input for the phase manifest generated at gate PASS. It is the context carried forward to the next phase's session. And it is the forensic record of what actually happened — including the deviations that the gate criteria did not cover.
::

## How Practitioners Apply It

::steps

### Define gate criteria before the sprint

Write specific, testable gate criteria for the phase before the sprint begins. Not "the feature works" — but: "the sidebar renders Introduction as a direct leaf item (no collapsible group wrapper)" or "unauthenticated GET to `/documentation/the-strata/stratum-1-classification` returns HTTP 200." Gate criteria that are vague at the start will be rubber-stamped at the end.

### Open the session with context

Provide the session context: requirement identity, phase number, plan reference, and context carried forward from all prior phases of this requirement. The copilot confirms understanding before generation begins. If the confirmation misrepresents scope or prior context, correct it before generation starts — this is the cheapest gate in the loop.

### Execute a complete sprint

All phase files are generated in one sprint — not file-by-file waiting for individual approval. The sprint produces a complete, coherent change set for the phase. Deviations are logged in `phase-n-notes.txt` as they occur — inline, not retrospectively.

### Run all four automated checks

After the sprint: `npm run type-check` (zero TypeScript errors), `npm run lint` (zero ESLint errors/warnings), `npm run test:unit -- --run` (all tests passing, no new skips without reason), `npm run build` (production build succeeds). Any failure is resolved by the copilot before the manual gate opens.

### Human tests against gate criteria

Exercise the feature as a user would. Include error paths and edge cases. Check for regressions in adjacent features. For UI changes, verify visual correctness, interaction behavior, and responsive states. The gate criteria are the minimum scope — observations beyond the defined criteria are captured as deviation entries (if issues found) or context-forward items (if architectural discoveries made).

### Generate artifacts on PASS

On gate PASS: generate `PHASE-N-IMPLEMENTATION-MANIFEST.md` (the formal, structured record of what was built), append an entry to `RUNNING-COMMIT-LOG.md` (the cumulative commit narrative for this requirement), and finalize the `CONTEXT TO CARRY FORWARD` section of `phase-n-notes.txt`. These artifacts are not administrative overhead — they are the mechanism by which the truth of what was built propagates into the next phase.

::

## What Goes Wrong Without It

**The human becomes an approver, not a gate.**

Automated tests pass. The build succeeds. The output looks reasonable. The engineer marks the phase complete and moves to the next one. The feature deploys. The customer experience fails in an edge case the tests did not cover, because the tests verified code behavior and the gate was supposed to verify user behavior.

**Prior-phase context is lost.**

A new session opens without context from prior phases. The copilot generates against its training defaults and the current conversational context. A pattern established in Phase 2 is "refactored" in Phase 5 by a session that has no context for why it was established. The refactor introduces a subtle regression. The regression surfaces in Phase 8. By then, the connection between the Phase 5 refactor and the Phase 8 symptom is invisible.

**Deviations go undocumented.**

Phase N encounters an architectural finding. The finding is handled. The fix works. No one documents it. Phase N+2 introduces code that conflicts with the undocumented decision. The conflict surfaces as a bug with no traceable cause — no deviation log entry, no context-forward note, no phase manifest describing why the code was structured the way it was.

## StrataProtocol.org Example

This very page. REQ-001, Phase 2 is executing the Execution Loop right now.

The phase has a defined scope (six content files in `content/documentation/the-strata/`), defined gate criteria (all seven required sections present on each stratum page, unauthenticated access returns HTTP 200 for all six pages, all internal links resolve without 404), and a `phase-2-notes.txt` tracking decisions and deviations as they occur.

The session manifest generated at gate PASS will be `PHASE-2-IMPLEMENTATION-MANIFEST.md`. The running commit log entry will be appended to `RUNNING-COMMIT-LOG.md`. The full history of this platform's construction — every phase of every requirement, with phase notes and manifests — lives in `docs/strata/4.execution-loop/sessions/`.

The Execution Loop's self-referential proof: the claim that this platform was built using STRATA is verifiable, not just asserted, because the loop produces a durable record of every phase.

::u-page-c-t-a
---
title: Go deeper with a member account
description: "The Stratum 4 deep-dive covers the complete Execution Loop protocol: session context templates, sprint execution guide, gate protocol checklists, artifact generation templates, deviation handling procedures, and the anti-pattern catalog."
variant: soft
links:
  - label: Access with a member account
    to: /documentation/deep-dives/
    icon: i-ph-lock
    color: neutral
---
::

## Related Strata

**← Previous:** [Stratum 3 — Authority Chain](/documentation/the-strata/stratum-3-authority-chain) — provides the mandate the Execution Loop operates inside. Every session loads the operative constitution; every sprint generates within the constraints the chain defines; every escalation follows the protocol the chain specifies.

**→ Next:** [Stratum 5 — Artifact Trail](/documentation/the-strata/stratum-5-artifact-trail) — collects every Execution Loop artifact into the permanent, durable record of the project's construction. Every phase note, every manifest, every commit narrative produced in Stratum 4 is a Stratum 5 artifact.
