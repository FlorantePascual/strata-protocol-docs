<!--
  PUBLIC / REDACTED SUMMARY of one edition of this project's operative constitution.
  It shows *what* the constitution governs and *why*, not the line-by-line rules that
  enforce it. The master and the deployed edition are maintained in the project's
  private repository. This is not a copy of either.
-->

# StrataProtocol.org — Operative Constitution (Stratum 3)

> **Edition:** Claude Code
> **Master:** `docs/strata/3.authority-chain/CLAUDE.md`, deployed to `/CLAUDE.md`
> **Constitution version:** 1.2. The constitution is versioned on its own; this number is not a STRATA Protocol version
> **Deployed sibling edition:** GitHub Copilot, summarized in [`copilot-instructions.md`](./copilot-instructions.md)
>
> This file is a public summary of the edition, not the edition itself.

## What this document is

The operative constitution is the last node of the **authority chain**: the versioned source of
truth that governs every AI coding session and human contribution on this codebase. In the STRATA model
it is Stratum 3's own output. The nodes above it (the vision and business case, the requirements, and
the standards) decide *what* is built and *why*; the constitution gathers what an AI agent must know from
all of them into the context the agent loads at the start of every session. Below it is the execution
loop, where the work actually happens. Everything an agent does is bounded by, and traceable to, this
document.

## One constitution, issued in editions

Each AI agent loads its instructions from its own path and in its own format. So the constitution is
**one node issued as several editions**, one per agent, each written in that agent's native form.

- An **edition** is the constitution as one agent loads it. This project deploys two: this Claude Code
  edition, deployed to `/CLAUDE.md`, and the GitHub Copilot edition, deployed to
  `.github/copilot-instructions.md`.
- A **master** is the authored copy of an edition, held under `docs/strata/3.authority-chain/`. Masters
  are the only copies edited. The **deployed edition** is cut from its master unchanged.
- **Fidelity** holds between a master and its deployed copy: deployment copies, it never edits.
- **Parity** holds between editions: every edition imposes the same mandate. The wording may differ
  because the format differs. What the agent is permitted to do does not.

No edition outranks another. When two editions disagree, both are checked against the nodes above
them, and the one that contradicts an upstream node is the defect.

This edition is native to Claude Code: it composes the project's private operational manuals through
Claude's `@import` mechanism, and a companion rules file carries the content-authoring detail. Both are
maintained in the private repository.

## A bounded mandate

The copilot's authority is intentionally limited. It implements against the active Feature Release and
its approved phase plan. It does not invent scope, redesign architecture, or alter the security posture
on its own initiative. Where a situation is ambiguous or unprecedented, the standing rule is to
**escalate, not decide** — the human stays the gate.

## What it governs

The constitution is organized around a fixed set of concerns. At a high level, it fixes:

- **Identity & context** — what the product is, how it is deployed, and the classification that sets
  its engineering posture.
- **Stack discipline** — a governed, closed set of dependencies and a strict separation between the
  application, shared, server, and content layers. Agents work *within* the set rather than extending it.
- **Hard prohibitions** — a small, categorical list of actions that are wrong regardless of context,
  protecting the deployment model, the content architecture, and the security boundary.
- **Security invariants** — above all, the access-control rule: protected content is enforced at more
  than one independent layer, and no single layer is sufficient on its own. This is the project's most
  important invariant.
- **Content & authoring standards** — how documentation is structured, ordered, and written, so the
  published site stays consistent, navigable, and on-voice.
- **Engineering standards** — typing, testing, and output discipline: declare scope before generating,
  verify before yielding, and log every deviation explicitly.
- **Escalation protocol** — the defined way to stop and surface a decision rather than guess at one.

## Why it exists

This is what makes AI-assisted output **auditable**. Because the constitution is explicit, versioned,
and loaded at the start of every session, any line of generated code can be traced back to the rule
that permitted it and to the Feature Release that motivated it. It is the reason the execution loop
produces a governed record instead of a black box — and the reason a project can let an AI move fast
without losing control of what it builds.

## Where this project stands

This project adopts the full edition model of STRATA Protocol v1.2 next. Today its two editions are
maintained as paired masters and edited together, in the same change. It does **not yet** keep a
register of its editions and their states, does **not yet** carry clause identifiers in its editions,
and does **not yet** check parity mechanically. The skeleton below is the model it is moving to, not a
description of its current files.

---

## The edition skeleton

Every edition of a constitution written to STRATA Protocol v1.2 follows the same section order, in its
agent's own format. This skeleton is generic. It names no project.

```
# Operative Constitution · [Project name]

> This file is your operative constitution for this project.
> Load it at the start of every session. Do not deviate from it.
>
> Edition:               [the agent this edition serves]
> Master:                docs/strata/3.authority-chain/[mirrored path]
> Constitution version:  v[N]. Sibling editions and their state:
>                        docs/strata/3.authority-chain/editions.md
> If you are reading the deployed copy, it is a copy. Edit the master and re-cut.

 1. Identity and mandate             a governed actor; its judgment advisory, the human's final
 2. Project context                  project, classification, objective, stack;
                                     the current release is supplied at the prompt
 3. Authority chain                  the documents above this one, in order of authority;
                                     authority runs downhill and never sideways
 4. Standards load on demand         which standards file to load for which kind of work
 5. Invariants that stop a release   the few rules no justification overrides
 6. Architecture constraints
 7. Code generation rules            the current Feature Release and phase only
 8. Interface rules                  pages, components, design tokens, accessibility
 9. Prose                            the writing standard for anything the agent writes
10. Testing
11. Escalation and deviations        when to stop and ask; where a deviation is recorded
12. Prohibited actions
13. Session anchor                   Feature Release, phase, plan path, chain version,
                                     context carried forward

Footer: the master's path. Do not edit a deployed copy.
```

**Clause identifiers.** Every mandate clause opens with a stable identifier, `MC-01`, `MC-02` and so
on. An identifier is never renumbered or reused, so identifiers need not run in section order. The same
clause carries the same identifier in every edition, which is what makes parity checkable rather than
asserted: at the point the constitution changes, each edition's identifiers are compared and the result
is reported. Anything true of one agent only sits in a marked tool-local section.

---

> The enumerated rules, exact constraints, and security mechanics behind each heading above are held in
> the private master of this edition and in the member toolkit. Detailed *public* standards that the
> constitution points to — such as the content file-structure standard — live under
> [`docs/strata/2.derivation/10-engineering-standards/`](../2.derivation/10-engineering-standards/).

_Operative constitution, Claude Code edition — public high-level summary. Master in the private
repository at `docs/strata/3.authority-chain/CLAUDE.md`, deployed to `/CLAUDE.md`._
