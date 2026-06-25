<!-- This is the Master Copy of `copilot-instructions.md`. Deploy it by copying to `.github/copilot-instructions.md`. -->
<!--
  PUBLIC / REDACTED COPY — high-level.
  This is a deliberately high-level view of the Stratum 3 authority chain for StrataProtocol.org.
  It shows *what* an operative constitution governs and *why* — not the line-by-line rules that
  enforce it. The full constitution (exact constraints, security mechanics, and operational
  reference) is maintained in the project's private repository.
-->

# StrataProtocol.org — Operative Constitution (Stratum 3)

## What this document is

The operative constitution is the **authority chain**: the single, versioned source of truth that
governs every AI coding session and human contribution on this codebase. In the STRATA model it sits
at Stratum 3 — beneath classification and derivation, which decide *what* is built and *why*, and above
the execution loop, where the work actually happens. Everything an agent does is bounded by, and
traceable to, this document.

## A bounded mandate

The copilot's authority is intentionally limited. It implements against numbered requirements and
approved phase plans. It does not invent scope, redesign architecture, or alter the security posture on
its own initiative. Where a situation is ambiguous or unprecedented, the standing rule is to
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
and consulted at the start of every session, any line of generated code can be traced back to the rule
that permitted it and the requirement that motivated it. It is the reason the execution loop produces a
governed record instead of a black box — and the reason a project can let an AI move fast without losing
control of what it builds.

---

> The enumerated rules, exact constraints, and security mechanics behind each heading above are held in
> the private copy of this constitution and in the member toolkit. Detailed *public* standards that the
> constitution points to — such as the content file-structure standard — live under
> [`docs/strata/2.derivation/10-engineering-standards/`](../2.derivation/10-engineering-standards/).

_Operative constitution — public high-level summary. Maintained as paired master copies for the
project's AI agents (`CLAUDE.md` and this `copilot-instructions.md`), each deployed to its agent's
instruction path._
