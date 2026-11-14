# `docs/strata` — Folder Template

This folder is a **template you can download**. It shows what the `docs/strata/` folder of a project governed by the [STRATA Protocol](https://strataprotocol.org) contains: one top-level folder per stratum, and inside them the slots each stratum fills.

It is populated **sparingly**, with historical artifacts from the development of `strataprotocol.org` itself. They are real governance documents, not samples written for this folder. The `2.derivation/04-prd/` folder holds a handful of mirror copies of requirement records derived during this platform's implementation, to illustrate the concept rather than to publish the whole set.

## What the artifacts are

**Most artifacts here are records from STRATA Protocol v1.0.** This platform was built under that version, which numbered its requirements `REQ-NNN`, so the populated records carry that shape: `REQ-000` and `REQ-001` in `04-prd/`, and the `REQ-000` execution session in `4.execution-loop/sessions/`. They are kept as they were written. They are history, and they are not rewritten to the current vocabulary.

**FR-001 is this platform's first release under STRATA Protocol v1.2.** STRATA Protocol v1.1 retired `REQ-NNN`: requirements are now `PRD-NNN`, and the unit the execution loop runs against is a Feature Release, `FR-NNN`. FR-001 published STRATA Protocol v1.2 across this platform. Its release record (`2.derivation/04-prd/FR-001-protocol-v1.2-publication.md`) and its session index (`4.execution-loop/sessions/FR-001-protocol-v1.2-publication/index.md`) appear here as placeholders. They show where a v1.2 record and its session sit, and that the session path mirrors the record's path, without publishing the release's internal decisions.

## Placeholders and `.gitkeep`

Not every slot is populated. Two markers show a slot that exists in the project but is not published here:

- **A placeholder file** stands in for a specific artifact that exists in the private project repository. It names the artifact, says what it holds, and states that it is not published. Signing in to strataprotocol.org does not reveal it either.
- **A `.gitkeep` file** keeps an empty folder in place. Git does not store empty folders, so the file is what makes the slot visible. The slot is part of the structure even where this public copy carries nothing in it.

## This folder

```
docs/strata/
├── 1.classification/            Stratum 1: the classification record
├── 2.derivation/                Stratum 2: vision, requirements, standards
│   ├── 01-vision/
│   ├── 02-current-system-assessment/        (.gitkeep)
│   ├── 03-brd/
│   ├── 04-prd/                  REQ-000, REQ-001; FR-001 (placeholder)
│   ├── 05-functional-specs/                 (.gitkeep)
│   ├── 06-nfr/                              (.gitkeep)
│   ├── 07-target-architecture/adr/, pdr/    (.gitkeep)
│   ├── 08-api-data-contracts/               (.gitkeep)
│   ├── 09-migration-strategy/               (.gitkeep)
│   └── 10-engineering-standards/
├── 3.authority-chain/           Stratum 3: summaries of two constitution editions
├── 4.execution-loop/            Stratum 4: execution sessions
│   ├── sessions/
│   │   ├── REQ-000-Platform-Bootstrap-PreFlight/   one session, shown whole
│   │   ├── REQ-001-Public-Framework-Documentation/ (.gitkeep)
│   │   └── FR-001-protocol-v1.2-publication/       (placeholder index)
│   └── spikes/                              (.gitkeep)
└── 5.artifact-trail/            Stratum 5: the artifact trail index
```

The folder names in `2.derivation/` are this platform's own, and they predate the protocol's later numbering. `10-engineering-standards/` is one example: engineering standards are document 11 at STRATA Protocol v1.2. Historical folders are not renamed, because the records inside them cite those paths.

## The expected tree at STRATA Protocol v1.2

A project starting today lays out Stratum 1 and Stratum 2 as below. It is one folder per derivation document, not one file, and a small project keeps one file per folder.

```
docs/
└── strata/
    ├── 1.classification/                    (Stratum 1 output)
    └── 2.derivation/                        (Stratum 2 output)
        ├── 01-vision/
        │     ├── project-manifesto.md       (optional, recommended — Authority Chain root)
        │     └── vision-business-case.md
        ├── 02-current-system/               (Class 2 only)
        ├── 03-brd/
        │     └── business-requirements.md
        ├── 04-prd/
        │     ├── README.md                  vocabulary · milestones · order · BR coverage
        │     ├── feature-releases.md        the build sequence, when 04 is split
        │     ├── PRD-[capability].md        one per capability, when 04 is split
        │     └── [capability]/
        │           └── [milestone]/
        │                 └── FR-NNN-[narration].md
        ├── 05-functional-spec/              (redirect stub when absorbed into 04)
        ├── 06-nfr/
        ├── 07-target-architecture/
        │     └── adrs/
        │           └── adr-001-[decision].md
        ├── 08-api-data-contracts/
        │     └── openapi.yaml
        ├── 09-transition-strategy/
        ├── 10-risk-dependency-analysis/
        ├── 11-engineering-standards/
        │     └── engineering-manifesto.md   (optional, recommended)
        ├── 12-testing-strategy/
        ├── 13-devops-deployment-plan/
        └── 14-master-implementation-plan/   (optional)
```

The other strata follow the same rule. `3.authority-chain/` holds the masters of the operative constitution, one per AI agent edition. `4.execution-loop/sessions/` holds one session folder per Feature Release, at a path that repeats the release record's path below `04-prd/`, so `04-prd/[capability]/[milestone]/FR-NNN-[narration].md` is executed in `sessions/[capability]/[milestone]/FR-NNN-[narration]/`. When document 04 is a single file, the capability and milestone segments drop out, as they do for FR-001 here.

## Learn more

- Framework documentation: [strataprotocol.org/documentation](https://strataprotocol.org/documentation)
- The five strata explained: [strataprotocol.org/documentation/the-strata](https://strataprotocol.org/documentation/the-strata), or in [`docs/protocol/the-strata/`](../protocol/the-strata/) in this repository
- The artifact trail of this platform, stratum by stratum: [`5.artifact-trail/README.md`](./5.artifact-trail/README.md)
