---
title: "Stratum 5 — Artifact Trail"
description: The artifact trail documents every decision, deviation, and sign-off — and outlives the project.
navigation:
  order: 5
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-24"
---

> *The artifact trail outlives the project. A system whose construction cannot be reconstructed from its records is a system that will be misunderstood by everyone who touches it next.*

Stratum 5 — Artifact Trail is the continuously assembled, living record of a STRATA-governed project — every decision made, every deviation encountered, every gate passed, every context carried forward, and every architectural choice justified. It is not written at the end of a project. It is assembled throughout, as a natural output of the governed acts performed across Strata 1 through 4.

By the time the last gate in Stratum 4 passes, the Artifact Trail is already substantially complete. What Stratum 5 defines is the **assembly discipline** — the rules that govern how individual artifacts are collected, structured, versioned, and made durable beyond any single development session, any single engineer, or any single project engagement.

::strata-diagram{name="strata_artifact_trail" alt="STRATA Artifact Trail — five stratum columns depositing their artifact categories into an accumulating trail band that spans the full project lifecycle"}
::

## Why This Stratum Exists

Code encodes *what* was built. The Artifact Trail encodes *why*.

Without the trail, a future engineer — or a future AI session — has no forensic record of why the system works the way it does. They "correct" what was deliberately designed. They repeat investigations already performed. They introduce patterns that conflict with workarounds established in prior phases, for reasons no longer visible in the code itself.

The trail serves three distinct purposes:

**Forensic** — The trail answers: *Why does the system work the way it does?* Every architectural decision has an ADR that records the context, options considered, and rationale for the choice made. Every deviation has a log entry that records what changed, why it diverged from the plan, and the downstream implication. This forensic record prevents the most expensive class of maintenance error: a future actor "correcting" something deliberately designed.

**Operational** — The trail serves as the authoritative context source for all future development on the same system, whether that future is one week or two years away. A new STRATA engagement extending an existing system does not begin with a blank slate — it begins with a complete Classification Record, a full derivation document set, a deviation log, and a commit trail narrated at the requirement and phase level.

**Methodological** — The trail is evidence that STRATA was followed. For a practitioner positioning STRATA to clients, employers, or a technical audience, a complete Artifact Trail for a shipped project demonstrates not just *what* was built, but *how every decision was made* and *how the AI was governed during construction*. That is proof of methodology, not a claim of it.

## Key Concepts

### Six Artifact Categories

Every artifact in the STRATA trail belongs to one of six categories:

| Category | Produced in | Contents |
|---|---|---|
| 1 — Classification | Stratum 1 | Classification Record; reclassification records if applicable |
| 2 — Derivation | Stratum 2 | The 14 normative documents — Vision, BRD, PRD, Architecture, Standards, etc. |
| 3 — Authority | Stratum 3 | Manifesto, UI & Testing Standards, operative constitution, authority chain version log |
| 4 — Execution | Stratum 4 | Phase notes, phase manifests, running commit log, deviation log, commit narratives |
| 5 — Decision | All strata | Architecture Decision Records (ADRs), Process Decision Records (PDRs) |
| 6 — Continuity | Project close | Project close record, system summary, future engagement brief, known debt register, trail index |

::note
Continuity artifacts (Category 6) are the bridge from the completed project to every future engagement. They are the documents a new engineer or new AI session reads *first* when beginning work on an existing STRATA-governed system — before touching the codebase, before reading the architecture document, before opening a session.
::

### Forensic Purpose

Every significant decision made on a STRATA project produces an Architecture Decision Record (ADR):

- **Context**: The situation that made the decision necessary — what forces were at play, what constraints existed
- **Options considered**: The alternatives evaluated and their trade-offs
- **Decision**: What was decided — stated clearly and unambiguously
- **Rationale**: Why this option over the others
- **Consequences**: What becomes easier or harder because of this decision; technical debt deliberately accepted
- **Review trigger**: The condition under which this decision should be revisited

The ADR answers "why was it built this way?" for any future engineer or AI session. Without the ADR, the future actor sees only the code — the *what* without the *why*. The most expensive class of maintenance error is a future actor "fixing" something that is the way it is for a documented reason that was invisible to them.

### Continuity Record

At project close, Stratum 5 produces the continuity artifacts — a structured set of documents that allow the next engagement to begin from current reality rather than re-discovering what the prior engagement learned:

- **Project close record**: Formal declaration that the project is complete — final state, outstanding items, and known future work
- **System summary**: Plain-language description of the system as built — what it does, how it is structured, where things are — written for a reader with no prior context
- **Future engagement brief**: What the next STRATA engagement needs to know before writing the first spec — what worked well, what to approach carefully, deviations with ongoing implications, recommended next classification
- **Known debt register**: Deliberate technical debt accepted during delivery — what it is, why it was accepted, and what the remediation path is
- **Trail index**: Navigation guide to the full Artifact Trail — a map of where everything is and what it covers

## How Practitioners Apply It

::steps

### Assemble as you go

Trail artifacts are produced during governed acts — not retrospectively. A trail assembled after the fact is reconstruction, not record. Commit session records when sessions close. Commit manifests and commit log entries at gate PASS. Log deviations when they are discovered — not at the end of the phase.

### Produce ADRs at the moment of decision

Context for an architectural decision is sharpest at the moment the decision is made. It degrades rapidly afterward. When an architecture decision is made during Stratum 2 derivation or during a Stratum 4 sprint, produce the ADR immediately — in the same commit as the change that implements the decision.

### Keep derivation documents current

When a deviation in the Execution Loop identifies a derivation document that requires updating, make that update before the next phase begins. Increment the document version and note the update in the deviation log entry that triggered it. A derivation document that has not been updated to reflect the system as built is a document that will mislead the next engagement.

### Produce continuity artifacts at project close

After the last gate passes, synthesize the trail into continuity artifacts: the project close record, system summary, future engagement brief, known debt register, and trail index. These are not administrative overhead — they are the documents that allow the next engagement to begin from current reality. The trail index is produced last, after all other continuity artifacts are complete.

::

::tip
**Key insight:** An ADR written a week after a decision is reconstruction, not record. The context that made the trade-offs obvious has already faded. Write the ADR in the same commit as the change that implements the decision — that is when the rationale is sharpest, the alternatives are still visible, and the constraints that shaped the choice are still fresh.
::

## What Goes Wrong Without It

The trail's absence makes the only record the codebase itself.

The codebase describes what was built. Without the trail, no one knows why. The next engagement makes decisions that contradict deliberate architectural choices — because those choices have no visible rationale. Workarounds are "fixed" into regressions — because the workaround was never documented as a workaround. Technical debt that was deliberately accepted and documented in a known debt register becomes invisible, then urgently surprising — because the register does not exist.

The authority chain loses its derivation history. Future sessions are governed by a constitution that has accumulated constraints over time, but has no version log explaining why each constraint was added. The copilot operates inside a mandate it cannot understand — and neither can the human engineer who inherited the project.

The most common failure mode: trail artifacts are deferred because delivery velocity feels more urgent. Phase notes are not committed until the end of the requirement. ADRs are never written because architecture decisions "seemed obvious." The commit log has commit messages like "fix stuff" and "more work." By the time the project closes, reconstruction is the only option — and reconstruction is not a trail. It is a best-effort approximation of one.

## StrataProtocol.org Example

The `docs/strata/` directory hierarchy is the complete Artifact Trail for this platform.

**Category 1**: `docs/strata/1.classification/classification-record.md` — the signed Classification Record establishing Class 1, Category 1.3 Platformization.

**Category 2**: `docs/strata/2.derivation/` — the 14 normative documents, including Vision & Business Case, BRD, PRD (with individual requirement files under `docs/strata/2.derivation/04-prd/`), Engineering Standards, and Testing Standards.

**Category 3**: `docs/strata/3.authority-chain/` and `.github/copilot-instructions.md` — the Manifesto, UI & Testing Standards, the operative constitution, and the version log.

**Category 4**: `docs/strata/4.execution-loop/sessions/` — session records, phase manifests, and running commit logs for every requirement that has been implemented. Phase notes for every phase, appended as iterations occurred. Commit narratives narrated at the requirement and phase level.

The trail is what makes the self-referential proof verifiable — any practitioner can audit whether STRATA was actually followed on this platform by reading the trail, not by trusting the claim. The trail is the difference between asserting the methodology and demonstrating it.

::u-page-c-t-a
---
title: Go deeper with a member account
description: "The Stratum 5 deep-dive covers the complete Artifact Trail: ADR and PDR formats, the trail assembly discipline, continuity artifact templates, the trail completeness checklist, and worked examples from the StrataProtocol.org trail."
variant: soft
links:
  - label: Access with a member account
    to: /documentation/deep-dives/
    icon: i-ph-lock
    color: neutral
---
::

## Related Strata

**← Previous:** [Stratum 4 — Execution Loop](/documentation/the-strata/stratum-4-execution-loop) — every Execution Loop artifact feeds directly into the trail. Phase notes, phase manifests, running commit log entries, deviation log entries, and commit narratives are all Category 4 artifacts produced naturally by the loop.

Stratum 5 is the final stratum — nothing follows it in the STRATA Protocol. When the last gate in Stratum 4 passes and the trail is complete, the project is closed. The trail outlives the project.
