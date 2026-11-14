# STRATA Protocol · Correction Recommendations

> **Status: all seven applied to the protocol as v1.2, 2026-08-31.** This
> register stays as the reasoning behind each change. Each finding names a
> location, quotes the text as it stood at v1.1, states why it was a defect and
> gives the replacement. The line numbers below are v1.1 line numbers and no
> longer resolve against the current text.

| Field | Value |
| --- | --- |
| Reviewed | `README.md`, `stratum1.md`, `stratum2.md`, `stratum3.md`, `stratum4.md`, `stratum5.md`, at the v1.1 text |
| Against | The protocol at v1.1, stamped 2026-08-06 |
| Trigger | The `REQ-NNN` to `PRD-NNN` retirement and the two tiers introduced below it, both v1.1 changes. This review checks whether the rest of the corpus followed them |
| Status | **Applied.** All seven, plus four extension instances found during application. See the note below |
| Ingested | **v1.2**, [`CHANGELOG.md`](../CHANGELOG.md) |
| Date | 2026-08-31 |

**Finding count: seven.** Six are residue from the retirement. One is a
stack-neutrality defect found while tracing the sixth.

**Four extension instances were corrected during application.** The register
named specific line numbers; three further verification blocks and three further
"requirement" tier errors carried the same two defects at lines it did not name.
Correcting only the named lines would have left each document internally
inconsistent. The extensions are itemized in the v1.2 changelog entry.

---

## Severity bands

| Band | Means |
|---|---|
| **Breach** | The text contradicts another passage in the same corpus. A reader following it produces an artifact that does not match the protocol |
| **Material** | The text is stale rather than contradictory, and following it produces a wrong path, identifier or tier |
| **Routine** | The text reads as an oversight and misleads nobody who has read the surrounding section |

---

## Summary

| # | Location | Finding | Band |
|---|---|---|---|
| 1 | `stratum5.md:164-166` | The artifact trail gives the session path as `sessions/req-[nnn]-[slug]/`, contradicting the mirror rule `stratum4.md:693` states | **Breach** |
| 2 | `stratum4.md:479` | The full-loop diagram tells the human to supply `REQ` as session context, contradicting the SESSION CONTEXT block at `stratum4.md:143` | **Breach** |
| 3 | `stratum5.md:166`, `stratum5.md:168`, `stratum4.md:657` | "requirement" is used for the gated unit, which is now the Feature Release. `requirement` names the tier above | **Material** |
| 4 | `stratum4.md:151` | "all prior phases of this requirement", inside a block whose first line says `Feature Release:` | **Material** |
| 5 | `README.md:131` | "per-requirement development" in the headline description of the execution loop | **Routine** |
| 6 | `stratum5.md:515-519` | The artifact count table lists rows by file type but not by release, so an auditor cannot check coverage against the build order | **Routine** |
| 7 | `stratum4.md:205-208`, `stratum4.md:489`, `stratum4.md:908-911` | Automated verification is specified as npm script names and one stack's build order, in a protocol that ships seven template sets | **Material** |

---

## Finding 1 · The artifact trail contradicts the mirror rule

**Band: Breach.**

**Location.** `stratum5.md:164`, `:165`, `:166`, in the Category 4 artifact table.

**Current text.** Three rows give the location as:

```
docs/strata/4.execution-loop/sessions/req-[nnn]-[slug]/implementation/
```

**Why it is a defect.** `stratum4.md:693` states the rule this violates, and
states it as a rule rather than a convention:

> **The session path mirrors the derivation path.** The suffix after `sessions/`
> is identical to the suffix after `04-prd/`, so a specification and its
> execution record are a mechanical path substitution in either direction. No
> index lookup and no naming convention to remember.

`stratum4.md:697` and `stratum3.md:635` both give the resulting shape:

```
docs/strata/4.execution-loop/sessions/{capability}/{milestone}/FR-{NNN}-{narration}/
```

**Stratum 5 is the document an auditor reads to find files.** A wrong path in
the artifact trail is more expensive than a wrong path anywhere else in the
corpus, because it is the one place a reader is not expected to already know the
answer. The three rows also carry a retired identifier prefix, so a reader
following them produces both the wrong shape and the wrong tier.

**Proposed replacement.** In all three rows:

```
docs/strata/4.execution-loop/sessions/{capability}/{milestone}/FR-{NNN}-{narration}/implementation/
```

**Add one sentence below the table**, so the rule is stated where it is used
rather than only where it is derived:

> The path after `sessions/` is the path after `04-prd/`. Stratum 4 states the
> mirror rule; this table follows it rather than restating it.

---

## Finding 2 · The loop diagram asks for a retired identifier

**Band: Breach.**

**Location.** `stratum4.md:479`, inside the SESSION OPEN box of the full
end-to-end diagram.

**Current text.**

```
│  │  Human provides session context (REQ, phase, plan ref,  │
```

**Why it is a defect.** `stratum4.md:143`, in the same document, defines the
block the human actually supplies:

```
Feature Release: FR-[NNN]-[narration]
Phase:          Phase [N] — [Phase name]
Plan:           ./implementation-plan.md
```

The diagram is the most-copied artifact in Stratum 4. It is reproduced in
slides, in onboarding material and in the operative constitution editions of
downstream projects, and it is read by people who never reach line 143.

**Proposed replacement.**

```
│  │  Human provides session context (Feature Release,       │
│  │  phase, plan ref, context from prior phases)            │
```

---

## Finding 3 · "requirement" names the wrong tier

**Band: Material.**

**Locations.** `stratum5.md:166`, `stratum5.md:168`, `stratum4.md:657`.

**Current text.**

- `stratum5.md:166`: "Cumulative **per-requirement** log of all phase commit entries"
- `stratum5.md:168`: "Current position of **every requirement** across phases"
- `stratum4.md:657`: "tracks the current position of **every requirement** across phases"

**Why it is a defect.** This is not the same error as findings 1 and 2. Those
carry a retired prefix. These carry a live word pointing at the wrong tier.
`README.md:272` established three tiers where there was one:

| Tier | Identifier | Gated? |
|---|---|---|
| Product requirement | `PRD-NNN` | No. One verifiable statement |
| **Feature Release** | **`FR-NNN`** | **Yes. The unit the execution loop runs against** |
| Milestone | `M{N}` | No. A group that reaches a demonstrable increment |

A running commit log is per release, not per requirement. A release satisfies
several requirements, and `stratum2.md` §"The unit the Execution Loop runs
against" is explicit that a single verifiable statement is not something a human
demonstrates and signs off. So "per-requirement log" describes a file that the
protocol does not produce.

The execution state document has the same problem twice over: it tracks releases
through phases, and requirements have no phases to be positioned in.

**Proposed replacement.**

- `stratum5.md:166`: "Cumulative per-release log of all phase commit entries"
- `stratum5.md:168`: "Current position of every Feature Release across phases"
- `stratum4.md:657`: "tracks the current position of every Feature Release across phases"

---

## Finding 4 · The session context block contradicts itself four lines in

**Band: Material.**

**Location.** `stratum4.md:151`, inside the SESSION CONTEXT block.

**Current text.**

```
Context from prior phases:
  [Decisions, deviations, and architectural changes from all prior
  phases of this requirement that affect current scope.
```

**Why it is a defect.** The block's own first line is
`Feature Release: FR-[NNN]-[narration]`. A reader filling the template has
already named the release, then is asked for context from the phases of a
requirement, which has none.

**Proposed replacement.**

```
Context from prior phases:
  [Decisions, deviations, and architectural changes from all prior
  phases of this release that affect current scope.
```

---

## Finding 5 · The README's one-line description of the loop

**Band: Routine.**

**Location.** `README.md:131`.

**Current text.**

> _Phased, per-requirement development. No phase advances without human sign-off._

**Why it is a defect.** It is the caption under the execution loop heading, so
it is the first definition of the loop a new reader meets, and it names the
wrong unit. Low severity because the section below it is correct.

**Proposed replacement.**

> _Phased, per-release development. No phase advances without human sign-off._

---

## Finding 6 · The artifact count table cannot be checked against the build order

**Band: Routine.**

**Location.** `stratum5.md:515-519`.

**Current text.** Rows count artifacts by file type, with a single `sessions/`
path for all of them:

| Phase in-flight records (`phase-[n]-notes.txt`) | [N] | `docs/strata/4.execution-loop/sessions/` |

**Why it is a defect.** This is a completeness check that cannot detect the
failure it exists to catch. A project with ten releases and nine session folders
reports a plausible count for every row. The build order at
`04-prd/feature-releases.md` states one total order, and coverage is checkable
against it release by release.

**Proposed addition**, rather than a replacement. Keep the type counts and add
one table:

| Release | Session folder | Phases gated | Manifests | Commit log |
|---|---|---|---|---|
| `FR-NNN` | {present \| absent} | {N of M} | {N} | {present \| absent} |

> One row per release in the build order. A release with gated phases and no
> manifest is the gap this table exists to surface.

---

## Finding 7 · Automated verification is specified in one stack's vocabulary

**Band: Material.**

**Locations.** `stratum4.md:205-208`, `stratum4.md:489`, `stratum4.md:908-911`.

**Current text.**

```
[ ] npm run type-check    — zero TypeScript errors
[ ] npm run lint          — zero ESLint errors, zero warnings
[ ] npm run test:unit     — all tests passing, no new skips without reason
[ ] npm run build         — production build succeeds cleanly
```

and, in the loop diagram at `:489`:

```
│  │  Types → stores → composables → components → tests │ │
```

**Why it is a defect.** The protocol is stack-neutral by construction. It ships
template sets for seven stacks, and `stratum1.md` classifies projects without
reference to a language. This passage fixes four npm script names, one package
manager, one type checker, one linter and one component model as the normative
gate.

The consequences are concrete and already visible in the template tree:

- Three sets use `pnpm`, not `npm`. One uses `pnpm check` rather than
  `type-check`, and one has no `test:unit` script at all because its meaningful
  suite is integration against a real database.
- `Types → stores → composables → components → tests` is Vue. A Payload project
  builds collections before services before routes. A backend project builds
  migrations before repositories before handlers. Neither reads as a variation
  of the quoted order.
- Every template set has silently rewritten this list in its own
  `{STACK_CHECKS}` and verify commands, which means the protocol's normative
  text is being routed around rather than applied. **A standard that every
  implementation overrides is not a standard.**

**Proposed replacement.** State the property, not the command.

```
[ ] Static type check      — zero errors
[ ] Lint                   — zero errors, zero warnings
[ ] Automated test suites  — all passing, no new skips without a
                             recorded reason
[ ] Production build       — succeeds cleanly
```

> **The commands are project-level, not protocol-level.** Each project names its
> own in the operative constitution or the session params, and the gate is that
> all four properties hold, whatever the scripts are called. A project whose
> meaningful suite is integration against a real database substitutes it here
> rather than reporting a unit suite it does not run.

**And for the diagram at `:489`**, replace the Vue-specific ordering with the
principle `stratum4.md:116` already states in stack-neutral terms:

```
│  │  Infrastructure before consumption, per the phase   │ │
│  │  ordering rule                                      │ │
```

**Keep the worked examples.** `stratum4.md:575-577` uses VueFlow to illustrate
deviation categories, and a worked example is allowed to be concrete. The defect
is in the normative checklist, not in the illustration.

---

## Not defects

Two `REQ-` occurrences are deliberate and correct. Neither should be removed.

| Location | Text | Why it stays |
|---|---|---|
| `README.md:272` | "**`REQ-NNN` is now `PRD-NNN`**" | The change record. Removing it erases the reason the retirement happened |
| `stratum2.md:262` | "`REQ-` is ambiguous once more than one document holds numbered requirements" | The rationale. It is what stops the prefix being reintroduced by someone who thinks it was arbitrary |

---

## Applying these

Findings 1 through 5 are text substitutions and can be applied in one pass.
Finding 6 adds a table. Finding 7 changes what the protocol requires, from four
named commands to four properties, and is the only one that needs a decision
rather than an edit.

**A companion audit covers the template tree**, which carries the same residue
in larger volume:
[`docs/_templates/req-to-fr-migration-recommendations.md`](../../_templates/req-to-fr-migration-recommendations.md).
