---
title: "Protocol Version History"
description: "Every version of the STRATA Protocol — what changed, why, how releases are numbered, and how to upgrade a project from STRATA Protocol v1.0."
status: stable
navigation:
  order: 1
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-11-14"
---

:status-badge{status="stable"}

The STRATA Protocol is versioned as a text. Each version is stamped when its text is final, and each version the public can cite is published as a numbered release, archived on Zenodo with a DOI of its own. This page records every protocol version, what changed in it, and what a project derived against an earlier version has to do to conform. It covers the framework only. The version in the site header belongs to the StrataProtocol.org application. It is a separate version, even when its number matches a protocol release, and it is reported on the [Version & Build](/documentation/version-build) page.

## Versions

| Protocol version | Release | Released | Version DOI | Status |
|---|---|---|---|---|
| STRATA Protocol :protocol-version{name="version"} | :protocol-version{name="release"} | :protocol-version{name="releaseDate"} | :protocol-version{name="releaseDoi"} | Current |
| STRATA Protocol :protocol-version{name="version" release="1.1.0"} | :protocol-version{name="release" release="1.1.0"} | :protocol-version{name="releaseDate" release="1.1.0"} | :protocol-version{name="releaseDoi" release="1.1.0"} | Superseded. Released retroactively from a reconstructed text |
| STRATA Protocol :protocol-version{name="version" release="1.0.0"} | :protocol-version{name="release" release="1.0.0"} | :protocol-version{name="releaseDate" release="1.0.0"} | :protocol-version{name="releaseDoi" release="1.0.0"} | Superseded |

A release date or version DOI that reads *pending* is recorded when that release is archived.

## How Releases Are Numbered

The protocol numbers its versions in two parts. A release is numbered in three, `MAJOR.MINOR.PATCH`, and its first two parts are always the protocol version it publishes: STRATA Protocol v1.0 was published as release 1.0.0. The third part counts publication-only revisions, such as a corrected page, a repaired public repository, or a metadata fix, re-released with the protocol text unchanged. A patch never changes the text. A change to the text arrives as a new protocol version, published as the next release ending in `.0`.

## What Changed in STRATA Protocol v1.2

STRATA Protocol v1.2 was stamped on 2026-08-31. It finishes a migration that protocol v1.1 started, when it retired the single requirement identifier in favor of product requirements and introduced two tiers below them, the [Feature Release](/documentation/reference) and the milestone. Six of the seven findings absorbed in this protocol version are places where the rest of the text had not followed that change. It is a new version rather than an edit because protocol v1.1 had already been stamped and cited: two different texts both calling themselves protocol v1.1 would be the stale-copy failure the [authority chain](/documentation/the-strata/stratum-3-authority-chain) exists to prevent.

- **Verification states properties, not commands.** The automated gate before manual review is four properties: a clean static type check, a clean lint, passing automated test suites, and a successful production build. Each project names the commands that check them. This is the only change in this protocol version that affects conformance.
- **The identifier migration is finished.** Session paths in the [artifact trail](/documentation/the-strata/stratum-5-artifact-trail) follow the mirror rule, the execution-loop diagram and the session context ask for the Feature Release, and "requirement" no longer names the gated unit where a Feature Release is meant.
- **Coverage is counted per release.** The artifact count gains one row per Feature Release, checked against the build order, because counting files by type cannot reveal a release with no execution record.
- **The loop no longer prescribes one stack's build order.** The sprint cites the protocol's stack-neutral phase-ordering rule instead.

## What Changed in STRATA Protocol v1.1

v1.1 was stamped on 2026-08-06 and applied in practice. It was released retroactively as 1.1.0; its overview is a reconstruction.

STRATA Protocol v1.1 absorbed 33 findings from the first full application of the protocol to a production project: a Class 1, Category 1.3 platformization build, delivered as a monorepo of eight applications across three ownership layers. Five findings were contradictions in the protocol v1.0 text. The rest were places where the text was consistent but did not fit practice.

- **Requirements and releases.** Product requirements are numbered `PRD-NNN`. The Feature Release is the gated unit of execution, and the milestone groups releases into a demonstrable increment. A business requirement maps to zero or more product requirements, and a discharge table makes the zero case visible.
- **The operative constitution.** Node 5 of the authority chain is one constitution issued as one edition per AI agent, kept true to its masters by fidelity and consistent across editions by parity. The constitution set is declared on the Classification Record, and each edition carries a state.
- **Planning at session open.** The analysis and the phased implementation plan move from derivation to the [execution loop](/documentation/the-strata/stratum-4-execution-loop), written when a release's session opens. The session path mirrors the derivation path.
- **The derivation tree.** Each derivation document gets its own folder. A depth directive on the Classification Record overrides the class default per layer. Layer 6 is Transition, in both directions. Document 05 may be absorbed into document 04, document 14 becomes an optional Master Implementation Plan, every authority-chain node names a source document, and two manifestos sit at different nodes.
- **Corrections to the protocol v1.0 text.** Documents 10 and 11 had been swapped, and document 01 carried three names.

## Upgrading from STRATA Protocol v1.0

Release 1.1.0 and release 1.2.0 are both minor increments, and neither is a drop-in revision. Protocol v1.1 is the version that broke conformance with protocol v1.0: it retired requirement identifiers, redefined node 5, and moved the planning documents between strata. Protocol v1.2 then restated the verification gate. The protocol does not yet define when its major version increments, so read a release number as the mapping above, not as a compatibility promise.

The protocol's own changelog tells a project on protocol v1.1 what to do. The steps below extend that back to protocol v1.0, the version this site published until release 1.2.0. Every change that alters what the protocol requires appears once, with the action it implies, ordered by conformance impact. A project already on STRATA Protocol v1.1 needs only the first step.

::steps
### Restate the verification gate as four properties

Record the gate as its four properties: static type check, lint, automated test suites, and production build. Name the commands that check them in your operative constitution or your session parameters. If your meaningful suite runs against a real database, report that suite, not a unit suite you do not run. If your commands were already the ones the earlier protocol text named, the record changes and the behavior does not.

### Number product requirements and execute Feature Releases

Stop issuing `REQ-NNN` identifiers. Number product requirements `PRD-NNN`, execute against Feature Releases (`FR-NNN`), the gated unit, and group releases into milestones where a demonstrable increment needs a name. Records already written under the retired scheme stay as history. Map each business requirement to zero or more product requirements, and add a discharge table so that a business requirement with none is visible rather than missing. An identifier may carry a domain segment, segmented by what is stable, never by what the project is designed to change.

### Reissue node 5 as one constitution in editions

Declare the constitution set on the Classification Record: one edition for each AI agent in use. Author a master for each edition under `docs/strata/3.authority-chain/editions/`, in a tree that mirrors the deployed paths, and deploy by copying, never by editing. Check fidelity (each deployed edition matches its master) and parity (every edition imposes the same mandate), record each edition's state, and stamp each deployed edition with the constitution version it was cut from. The stamp carries the most weight when your `docs/` tree lives outside the code repository. While you are in the authority chain, name a source document for every node, and give the project manifesto and the engineering manifesto each a named author and a home.

### Plan at session open, on the mirrored path

Stop writing a per-requirement analysis and phased implementation plan during derivation. Write both in the execution loop when a Feature Release's session opens, against everything learned so far. Name session folders by the mirror rule, where the path after `sessions/` equals the path after `04-prd/`, and rename existing folders to that shape.

### Restructure the derivation tree

Give each derivation document its own folder, `NN-{name}/`, with the files inside named for their content. Check the numbering: document 01 is the Vision & Business Case, document 10 is the Risk & Dependency Analysis, document 11 holds the Engineering Standards, and document 12 stays separate from document 11. Flow quality in three stages: business impact in document 03, detailed targets in document 04, and consolidation in document 06. Record a depth directive on the Classification Record that overrides the class default per layer, each with a stated reason. Treat Layer 6 as Transition both ways, inbound migration and outbound handover. Document 05 may be absorbed into document 04 if a redirect stub stays. Document 14, the Master Implementation Plan, is optional, and when written it sequences milestones.

### Apply the split-register rules if document 04 is split

These rules apply only when the product requirements document is split into several files. The register becomes two files: a build sequence with one row per Feature Release, and a README with the vocabulary, the milestone grouping, the ordering rationale, and the coverage check. State one total build order. The Feature Release identifier is its step number, fixed before the first commit carries it. Where dependencies leave the order free, order by what is irreversible, and size milestones as contiguous runs of the sequence. The platform foundation takes a release of its own, first in the sequence and tagged `enabling`. Write every capability document before any Feature Release record.
::

## Citing a Specific Release

To cite the framework or one of its releases, see [Cite the STRATA Protocol](/documentation/citation). The concept DOI always resolves to the latest release, and each version DOI resolves to its own release only.

::note
The canonical record of every protocol version, with the reasoning behind each change, is [`docs/protocol/CHANGELOG.md`](https://github.com/FlorantePascual/strata-protocol-docs/blob/main/docs/protocol/CHANGELOG.md) in the public repository. This page is a summary of it for readers of this site.
::
