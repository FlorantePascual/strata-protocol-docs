# STRATA Protocol · Changelog

**One home for the protocol's version history.** Each entry states what changed,
why, and what a project already derived against the previous version has to do
about it. Newest first.

Versions are two-part. There is no patch level: an editorial correction to a
stamped version still increments the minor, because the stamp is what a
downstream project cites when it records which text it derived against.

| Version | Date | Status | Findings absorbed |
|---|---|---|---|
| **v1.2** | 2026-08-31 | Current | 7, from [`03-protocol-correction-recommendations.md`](change-history/03-protocol-correction-recommendations.md) |
| v1.1 | 2026-08-06 | Superseded | 33, from [`02-refinements.md`](change-history/02-refinements.md) and [`01-recommendations.md`](change-history/01-recommendations.md) |
| v1.0 | — | Superseded | The published baseline |

---

## v1.2 · 2026-08-31

**v1.2 finishes a migration v1.1 started.** v1.1 retired `REQ-NNN` in favor of
`PRD-NNN` and introduced two tiers below it, the Feature Release and the
milestone. Six of the seven findings here are places the rest of the corpus did
not follow that change. The seventh is unrelated and older.

**Why this is a version increment and not an edit.** v1.1 was stamped in all six
files and cited by version number in at least one downstream project's approved
documents. Two different texts both calling themselves v1.1 is the parity failure
Stratum 3 warns about, where a stale copy loads and looks authoritative. Stratum
2 already states the rule this follows: a finalized document that must change
requires a documented reason and a version increment.

### Corrections to the v1.1 text

- **The artifact trail gave a session path that contradicted the mirror rule.**
  Stratum 5's Category 4 table located three execution artifacts under
  `sessions/req-[nnn]-[slug]/`, while Stratum 4 states that the path after
  `sessions/` is the path after `04-prd/`. All three rows now carry
  `sessions/{capability}/{milestone}/FR-{NNN}-{narration}/`, and the table
  carries one line pointing at the rule instead of restating it. Stratum 5 is
  where an auditor goes to find files, which makes it the most expensive place
  in the corpus for a wrong path.
- **The full-loop diagram asked the human for a retired identifier.** The SESSION
  OPEN box read `(REQ, phase, plan ref, ...)` while the SESSION CONTEXT block in
  the same document opens with `Feature Release:`. The diagram is the most-copied
  artifact in Stratum 4 and is read by people who never reach the block.
- **"requirement" named the gated unit in five places.** A running commit log is
  per release. Execution state tracks releases through phases, and requirements
  have no phases to be positioned in. Corrected in Stratum 5's artifact table,
  its execution-state row and its commit-narrative paragraph, in Stratum 4's
  execution-state description and deviation `Downstream` field, and in Stratum
  2's note on what document 14 can be assembled from.
- **The session context template contradicted itself four lines in.** It named
  the Feature Release on its first line, then asked for context from the prior
  phases "of this requirement".
- **The README's caption for the execution loop** said "per-requirement
  development".

### Structural changes

- **Automated verification states properties, not commands.** The gate was four
  npm script names, a type checker, a linter and one component model, in a
  protocol that classifies projects without reference to a language. It is now
  four properties: static type check, lint, automated test suites, production
  build. The commands are project-level and named in the operative constitution
  or the session parameters. A project whose meaningful suite is integration
  against a real database reports that suite rather than a unit suite it does not
  run.

  **This is the one change that affects conformance.** A project passing the v1.1
  gate is not automatically passing the v1.2 gate, and the reverse also holds.
  Nothing else in v1.2 changes what the protocol requires.

- **The sprint box in the loop diagram** no longer prescribes
  `Types → stores → composables → components → tests`, which is one framework's
  build order. It cites the phase ordering rule Stratum 4 already states in
  stack-neutral terms.

- **The artifact count table gained a per-release table.** Counting artifacts by
  file type cannot detect the failure the check exists to catch: a project with
  ten releases and nine session folders reports a plausible number on every row.
  Coverage is checkable only against the build order, one row per Feature
  Release.

### Found while applying, beyond the seven findings

The correction register named specific line numbers. Applying it faithfully
required four more instances of the same two defects, recorded here rather than
left inconsistent with the lines the register did name.

| Extension | Where |
|---|---|
| Three more stack-specific verification blocks | The `TESTING RESULTS` and `UPDATED TESTING RESULTS` templates, the `PRE-GATE STATUS` block, the phase-notes `Verified:` line, and the session protocol checklist |
| Three more "requirement" tier errors | Stratum 5's commit-narrative paragraph, Stratum 4's deviation `Downstream` field, Stratum 2's document 14 note |

### What a v1.1 project has to do

**Nothing, for findings 1 through 6.** They correct text that described artifacts
the protocol does not produce. A project that built the artifacts correctly was
already ignoring the wrong text; a project that followed it has session folders
to rename, and the mirror rule tells it the target shape.

**One thing, for the verification change.** A project citing v1.1 recorded four
npm script names as its gate. Under v1.2 it records the four properties and names
its own commands in its operative constitution or session parameters. If those
commands were already `npm run type-check` and the rest, the record changes and
the behavior does not.

**Two `REQ-` references remain and are deliberate.** The v1.1 change record below
and the rationale at Stratum 2 §Document 04 both name the retired prefix in order
to explain why it was retired. Removing either would erase the reason and invite
its reintroduction.

---

## v1.1 · 2026-08-06

v1.1 absorbs 33 findings from the first full application of the protocol to a production
project: a Class 1 category 1.3 platformization build, delivered as a monorepo of eight
applications across three ownership layers. Five were self-contradictions in the v1.0 text.
The rest are places the protocol was consistent and did not fit.

**Corrections to the v1.0 text**

- Documents 10 and 11 were swapped between the dependency map and the document sections. Document 10 is Risk & Dependency Analysis. Document 11 is Engineering Standards.
- Document 01 carried three names. It is the Vision & Business Case, and the Modernization Charter is a Class 2 section inside it.
- The Stratum 2 file path in prose disagreed with the tree beside it, in two files.
- The README and Stratum 2 disagreed on Layer 2's vocabulary.
- `00-manifesto.md` sat in the Stratum 2 tree with no author, no content definition and a number it did not need.

**Structural changes**

- **One folder per derivation, not one file.** `NN-{name}/`, with each file inside named for its content.
- **Depth is set on the classification record.** The class-keyed depth table is a default that the record overrides per layer with a stated reason. Categories, not just classes, drive depth.
- **Layer 6 is Transition and runs both ways.** Inbound migration and outbound handover. A greenfield project that hands its system to a recipient has real Layer 6 content.
- **`REQ-NNN` is now `PRD-NNN`**, and two tiers sit below it: the **Feature Release** as the gated unit, and the **milestone** as the group that reaches a demonstrable increment.
- **Requirement identifiers may carry a domain segment**, under one rule: segment by what is stable, never by what the project is designed to change.
- **The analysis and the phased implementation plan move to Stratum 4**, written when a session opens rather than months earlier.
- **The Stratum 4 session path mirrors the derivation path**, so a specification and its execution record are a mechanical substitution.
- **Quality flows in three stages**: business impact in document 03, detailed targets in document 04, consolidation in document 06. Every edge runs downhill.
- **Document 05 may be absorbed into document 04**, leaving a required redirect stub.
- **Document 12 does not merge into document 11.** Prescription and strategy split.
- **Document 14 is renamed Master Implementation Plan and is optional.** When written, it sequences milestones rather than individual releases.
- **Business requirement to product requirement is zero-to-many**, with a discharge table making the zero case visible.
- **Two manifestos, at different nodes**, each with a named author and a home.
- **Every Authority Chain node names a source document.**
- **Node 5 is the operative constitution, not `.github/copilot-instructions.md`.** That path was one vendor's convention standing in for the node itself, which broke on every project running Claude Code, Cursor or more than one agent at once. Node 5 is now **one constitution issued as one edition per agent**, because the agent formats are not interchangeable and a single file copied to all of them would be wrong in most. Masters sit under `3.authority-chain/` in a tree mirroring the deployed paths. **Fidelity** governs master to deployed copy, **parity** governs edition to edition, and parity is checked mechanically rather than maintained as a table. The constitution set is declared on the classification record.
- **The derivation output does not have to live in the code repository.** Three passages assumed it did. A project whose `docs/` tree ships to a documentation platform, or is deliberately kept out of the repo, is a supported topology, and it makes the version stamp on each deployed edition load-bearing rather than decorative.
- **Parity begins at Stratum 3, and the constitution set carries a state.** The set is declared at Stratum 1 and brought to parity at Stratum 3. Between those points the editions are unsynchronized by default, which is expected rather than a defect, and a session opened in that window is anchored to whatever its agent loaded rather than to a verified chain. Each edition records whether it is declared, authored, at parity or deployed. A deployed edition that is not at parity is the dangerous one, because it loads and looks authoritative.

**Organizing a split document 04**, all of it conditional on the split having happened. A
project that keeps one file needs none of it.

- **The register is two files.** A build sequence holding one row per Feature Release, and a README holding the vocabulary, the milestone grouping, the ordering rationale and the coverage check. An attribute lives in one of them and the other cites it.
- **The register states one total build order**, because `Depends on` gives only a partial one and the execution loop consumes a total one.
- **The Feature Release identifier is the step number**, fixed before the first commit trailer carries it. That is the only moment renumbering is free.
- **Where the partial order leaves freedom, order by what is irreversible**, and size milestones so each is a contiguous run of the sequence.
- **The capability partition leaves the platform foundation unowned.** It is not a capability, so it takes a release of its own, first in the sequence and tagged `enabling`.
- **Every capability document is written before any Feature Release record**, because a boundary is provisional until the document that consumes its reservations exists.

The findings register that produced this version, including which are defects and which are
refinements, is maintained separately by the practitioner who ran the engagement.

---

## v1.0

The published baseline. Superseded by v1.1.

---

_[StrataProtocol.org](https://strataprotocol.org/) · STRATA Protocol v1.2_
