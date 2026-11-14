# Stratum 3 — Authority Chain

The five-node authority chain that constitutionally governs every AI session.

*Rendered from [strataprotocol.org/documentation/the-strata/stratum-3-authority-chain](https://strataprotocol.org/documentation/the-strata/stratum-3-authority-chain) at STRATA Protocol release 1.2.0.*

> *The AI is governed, not autonomous. The authority chain defines what the copilot may do and how it must think. It operates inside a mandate — not outside one.*

Stratum 3 — Authority Chain is a five-node constitutional framework that governs every AI copilot session on a STRATA-managed project. It is not a prompt. It is not a style guide. It is a hierarchy of constraints, standards, and mandates — derived from the Stratum 2 document set — that constitutes the copilot's operating authority before the first line of code is generated.

Each node in the chain is derived from the one above it. Nothing in a lower node may contradict a higher one. The chain is versioned and evolves as the project evolves, and every session is anchored to its current state.

![STRATA Authority Chain — hierarchical funnel of governance documents from Project Manifesto to the operative constitution, issued as one edition per agent](https://strataprotocol.org/lib/diagrams/strata_authority_chain.svg)

## Why This Stratum Exists

An AI copilot operating without an authority chain generates against its own training defaults — the last conversation context, its training priors, or nothing at all.

The output may be syntactically correct and functionally plausible. But it is architecturally inconsistent with the project's specific requirements, risk profile, and established standards. It makes assumptions about stack choices that contradict documented decisions. It interprets "clean code" according to training defaults, not the project's engineering standards. It resolves ambiguities silently rather than surfacing them for human judgment.

> [!WARNING]
> A copilot operating without an authority chain is not a governed partner — it is a highly capable guesser. The authority chain converts the copilot from a guesser into a governed actor: one that generates within a defined mandate, escalates ambiguities, and defers architectural judgment to humans.

The authority chain eliminates this class of failure by replacing training defaults with a defined, project-specific mandate. The difference between those two states is the difference between AI-assisted development and AI-governed development.

## Key Concepts

### Five-Node Chain Structure

The authority chain is a hierarchy of five nodes, each derived from the nodes above it, and every node names a source document:

```
Project Manifesto
  → Vision & Business Case
    → Requirements
      → Standards
        → Operative constitution   ← one node, issued as one edition per AI agent
```

| Node | Source document |
|---|---|
| 1 — Project Manifesto | `2.derivation/01-vision/project-manifesto.md` |
| 2 — Vision & Business Case | `2.derivation/01-vision/vision-business-case.md` |
| 3 — Requirements | `2.derivation/03-brd/` and `2.derivation/04-prd/` |
| 4 — Standards | `2.derivation/11-engineering-standards/`, rooted in the engineering manifesto, and `2.derivation/12-testing-strategy/` |
| 5 — Operative constitution | `3.authority-chain/`, one master per edition, each deployed to its agent's path |

**Node 1 — Project Manifesto**: The project's foundational declaration: what the project is for and what it refuses to become. Written once by the project author, updated rarely, and the highest authority in the chain. It is optional and recommended: a vision derived without one has no recorded statement of intent above it, and the chain starts a node short.

**Node 2 — Vision & Business Case**: The strategic framing of the initiative. Governs all requirements, architecture decisions, and scope judgments. Any requirement that cannot trace to the vision is a candidate for removal.

**Node 3 — Requirements**: The business and product requirements from Stratum 2. Business requirements (`BR-NNN`) state what the business needs, product requirements (`PRD-NNN`) state what the system must do, and Feature Releases (`FR-NNN`) bundle product requirements into the units the copilot's execution loop runs against — one at a time, phase by phase. The analysis and the phased implementation plan are not written here: they are produced in Stratum 4, when the release's session opens.

**Node 4 — Standards**: Prescriptive standards for how the system looks, behaves, and is verified, drawn from the engineering standards and the testing strategy. The copilot may not deviate from these standards without explicit human instruction.

**Node 5 — Operative constitution**: The AI's operating mandate. It aggregates the essential constraints from all four upstream nodes into the form an agent loads at session start, and it is issued as one edition per AI agent in use.

The chain holds two manifestos, at different nodes. The **project manifesto** (node 1), written by the project author, constrains *what* gets built. The **engineering manifesto** (node 4), written by the engineering team, constrains *how* it gets built, and the prescriptive standards implement it. A chain that names one "Manifesto" at its root hides the second one entirely.

### One Constitution, Several Editions

Each AI agent loads its instructions from its own path, in its own format: Claude Code loads `CLAUDE.md`, GitHub Copilot loads `.github/copilot-instructions.md`, Cursor reads the `.cursor/rules/` tree, and several agents read `AGENTS.md`. These files cannot be one text copied to several paths, because the formats are not interchangeable. So node 5 is one constitution issued as several **editions**, one per agent, each written in that agent's native form. STRATA does not care which files an agent reads. It cares that every edition imposes the same mandate.

Four terms make that checkable:

- **Edition** — the constitution as one agent loads it. An edition may be a file or a directory.
- **Master** — the authored copy of an edition, held under `docs/strata/3.authority-chain/editions/` in a tree that mirrors its deployed path. Masters are the only copies anyone edits.
- **Deployed edition** — the copy at the agent's live path, cut from its master.
- **Constitution set** — every edition the project maintains, declared on the Classification Record and listed with its master and deployed path in `docs/strata/3.authority-chain/editions.md`.

Two invariants hold the editions together, and they are different invariants. **Fidelity** holds a deployed edition to its master: deployment copies and never edits, so an edit made at the deployed path is a fork, and a silent one, because the file still loads. **Parity** holds the editions to each other: the wording differs because the format differs, but what the agent is permitted to do does not. Fidelity is mechanical and cheap to check. Parity is the one that decays, so it is checked, never asserted: every mandate clause carries a stable identifier in every edition, and coverage is checked whenever the constitution changes.

The constitution set carries a state for each edition, not just a list:

| State | Meaning |
|---|---|
| **Declared** | Named on the Classification Record. Nothing has been authored |
| **Authored** | A master exists. Parity with its siblings has not been checked |
| **At parity** | Checked against the clause list and passing. Stratum 3 is not complete until every declared edition reaches this state |
| **Deployed** | Cut from its master and stamped with the constitution version it was cut from |

No edition outranks another. When two editions disagree, both are checked against nodes 1 to 4, and the edition that contradicts an upstream node is the defect. Where both are consistent with upstream and merely differ, the difference is a parity defect, and a human resolves it.

> [!NOTE]
> Where the chain is versioned is a project decision, and it is not always the code repository. A project whose `docs/` tree lives outside the repository keeps its masters with the derivation output and its deployed editions in the repository, so nothing in the repository can tell a current edition from a stale one. Every deployed edition therefore carries the constitution version it was cut from. Without that stamp, a stale edition is indistinguishable from a fresh one.

### What Every Edition Must Be

Every edition of the operative constitution has the same critical properties:

- **Written in the second person** — speaks directly to the AI ("You are...", "You must...", "You may not...")
- **Imperative throughout** — no suggestions, no preferences, no maybes
- **Self-contained** — an agent loading its edition cold has everything needed to operate correctly on this project
- **Native to its agent** — uses that agent's own format and loading semantics; anything true only of that agent sits in a marked tool-local section
- **Clause-identified** — every mandate clause carries a stable identifier, so parity is checkable rather than assertable
- **Explicit about escalation** — states clearly what the AI must do when it encounters ambiguity, scope questions, or situations not covered by the chain
- **Versioned** — every material change is committed with a clear change note, brings every edition back to parity in the same change, and re-cuts every deployed copy from its master

Each agent loads its deployed edition automatically from its own path, which makes the chain operative without manual injection every session. Those paths are owned by the tool vendors and will change. When one moves, the master tree is re-mirrored, and nothing about the mandate changes.

### Governed vs. Autonomous AI

A copilot operating inside an authority chain is a **governed actor**. It generates within a defined mandate, escalates ambiguities rather than resolving them silently, and defers architectural judgment to humans. The scope of its authority is explicit and bounded.

A copilot without an authority chain is an **autonomous actor** — optimized for helpfulness, generating against its own priors, making assumptions that seem reasonable in the moment and compound into architectural inconsistencies over time.

The authority chain does not make the copilot less capable. It makes the copilot's capabilities accountable: every output is generated inside a known mandate, traceable to a document in the chain, and reviewable against defined criteria.

## How Practitioners Apply It

### Build the chain before the first session

The authority chain is built before the first development session begins — not during development, not retrospectively after problems emerge. A copilot that opens its first session without an authority chain has already generated against training defaults for that session. A session opened before the editions reach parity is anchored to whatever its agent happened to load: keep its work, but record it as pre-chain work, not as work generated under the mandate.

### Write the manifestos

Two manifestos, by two authors. The project author writes the **project manifesto**: what the project is for, who it serves, what it will not do, and what must remain true. The engineering team writes the **engineering manifesto**: what the team believes, how it works, and what it does not compromise on. Register, tone, and principles — not implementation details. The project manifesto is the highest-authority document in the chain; everything downstream must be consistent with it.

### Reference Stratum 2 documents

Nodes 2 and 3 are derived from the Stratum 2 document set — the Vision & Business Case, the BRD, and the PRD with its Feature Releases. These documents do not need to be rewritten for the authority chain; they need to be referenced correctly and held to the chain's authority hierarchy.

### Write the standards

Prescriptive standards — not style preferences. They live in the engineering standards and the testing strategy, and they govern how all code on this project is written, how components are structured, how tests are co-located, what coverage is required, and what patterns are prohibited. The copilot must not deviate from these standards without explicit human instruction.

### Write one master per edition

Aggregate essential constraints from all four upstream nodes into one master for each declared edition, under `docs/strata/3.authority-chain/editions/`, in a tree that mirrors each deployed path. Structure each with: identity and mandate, project context, authority chain reference, stack constraints, code generation rules, testing requirements, output format requirements, escalation protocol, prohibited actions, and a session anchor. Write it in the second person, imperative throughout, self-contained, in its agent's native format, with an identifier on every mandate clause.

### Bring the editions to parity, then deploy

Check every edition against the clause list and record the result. Then cut each deployed edition from its master, unchanged, to the path its agent loads, stamped with the constitution version it was cut from, and record each edition's state in `editions.md`. Stratum 3 is complete only when every declared edition is at parity.

### Version and maintain the chain

Track every material change in an authority chain version log. When an architecture finding logged during the Execution Loop identifies an upstream implication, update the masters in the same commit that addressed the finding — do not defer — bring every edition back to parity, and re-cut each deployed copy.

## What Goes Wrong Without It

**Authority chain drift** — the most insidious failure mode in AI-assisted development.

Early sessions may be coherent because the copilot has recent context. As the project grows and context windows rotate across sessions, the copilot's effective mandate diverges from the project's actual requirements. Each session produces output that is architecturally inconsistent with prior sessions. The codebase accumulates contradictions that compound with every new phase.

Specific consequences:
- A pattern established in Phase 2 is "corrected" in Phase 7 by a session that does not know why it was established
- Stack constraints documented in the architecture are ignored because they were not in the last session's context
- Escalation triggers are missed because the copilot resolved ambiguities silently that it should have surfaced
- One agent's edition is corrected and its siblings are not, so the project runs under different mandates depending on which tool opened the session
- Each new session's output requires more human review to catch inconsistencies that a functioning authority chain would have prevented

By the time the drift is apparent, it has accumulated across many phases. Resolving it requires a systematic audit of all generated code against the authority chain that should have governed it from the start — expensive work that the chain itself would have made unnecessary.

## StrataProtocol.org Example

This platform was built under STRATA Protocol v1.0, which numbered requirements `REQ-NNN`, and its records keep those identifiers; FR-001 is its first [Feature Release](https://strataprotocol.org/documentation/reference#feature-release) under STRATA Protocol v1.2.

This platform runs two AI agents, so its operative constitution is deployed as two editions: `CLAUDE.md` for Claude Code and `.github/copilot-instructions.md` for GitHub Copilot.

Each edition establishes the copilot's identity (governed development copilot for a Nuxt 4/Docus documentation portal with SSR authentication and RBAC), project context (Class 1, Category 1.3 — Platformization), and a comprehensive stack constraints table. Critical sections include:

- **RBAC integrity rule**: Four-step checklist for any page with `requiredRole` in its frontmatter — frontmatter, `PROTECTED_ROUTES`, test case, and passing unit tests — all required in the same generation pass
- **Never-do list**: Eight explicit prohibitions, including never adding `nuxt generate` (which would expose member content to unauthenticated users) and never modifying `content-guard.ts` without explicit human instruction
- **Escalation protocol**: Six trigger conditions for stopping and escalating to the human engineer before proceeding

Every session that generated code on this platform — from the platform bootstrap through every content requirement — operated inside this chain. The authority chain version log in `docs/strata/3.authority-chain/` records every material change and the reason for it.

**Go deeper with a member account**

The Stratum 3 deep-dive covers the complete Authority Chain templates for all five nodes, the Derivation Agent prompt for authority chain construction, worked examples across different project types, and the version log convention.

- [Access with a member account](https://strataprotocol.org/documentation/deep-dives/)

## Related Strata

**← Previous:** [Stratum 2 — Derivation](stratum-2-derivation.md) — provides the 14 normative documents that the authority chain is derived from. The project manifesto, the Vision & Business Case, the business and product requirements, and the engineering standards and testing strategy are the sources of Nodes 1 to 4 of the chain.

**→ Next:** [Stratum 4 — Execution Loop](stratum-4-execution-loop.md) — the Execution Loop runs inside the mandate established by the authority chain. Every sprint session opens with its agent's edition of the operative constitution already loaded; every gate decision is made by the human who is the gate this chain designates.
