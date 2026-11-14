# STRATA Protocol — Documentation Hub

> 🔒 **Note:** The source code for this project is hosted in a private repository to protect proprietary algorithms and intellectual property. This repository serves as the public documentation hub, architecture overview, and issue tracker for the application.

**STRATA Protocol** is a governed framework for structured, traceable, and repeatable AI-assisted software delivery. It defines five layers of software project governance — Classification, Derivation, Authority Chain, Execution Loop, and Artifact Trail — that together ensure AI-generated output remains auditable, aligned, and production-safe.

**Current version:** STRATA Protocol v1.2, published in this repository as release 1.2.0.

**Live documentation:** [strataprotocol.org](https://strataprotocol.org)

---

## What's in this repository

This repository contains the publicly available framework documentation and governance artifacts for the STRATA Protocol:

- **[`docs/protocol/`](./docs/protocol/)** — The protocol itself: the canonical overview ([`README.md`](./docs/protocol/README.md)) and version history ([`CHANGELOG.md`](./docs/protocol/CHANGELOG.md)), the findings register behind the current version ([`change-history/`](./docs/protocol/change-history/)), and the five stratum pages rendered for reading on GitHub ([`the-strata/`](./docs/protocol/the-strata/)). [`PROVENANCE.md`](./docs/protocol/PROVENANCE.md) states where each file comes from
- **[`content/`](./content/)** — The public pages of [strataprotocol.org](https://strataprotocol.org) in their source form: getting started, core concepts, the five strata, reference material, the changelog, and the case studies. A page available only to members is represented by a short placeholder that describes it and links to it on the site
- **[`content/case-studies/`](./content/case-studies/)** — Four projects delivered through the full five-stratum sequence, across different classes, stacks, and eras. StrataProtocol.org is one of them, and its case study links the governance artifacts in `docs/strata/`
- **[`docs/strata/`](./docs/strata/)** — A folder template showing what a governed project's `docs/strata/` contains, populated with historical artifacts from the development of strataprotocol.org itself
- **[`docs/_templates/`](./docs/_templates/)** — The implementation-template sets, with overviews of the prompt pack that drives each one

Implementation templates, AI prompt guides, and the full developer guide are available to members at [strataprotocol.org](https://strataprotocol.org).

## Versions

Each release of this repository is archived on Zenodo under one concept DOI, which always resolves to the latest release. Release numbers are `MAJOR.MINOR.PATCH`: `MAJOR.MINOR` is the protocol version, and `PATCH` counts revisions to this repository that leave the protocol text unchanged.

| Release | Protocol | Notes |
|---|---|---|
| **1.2.0** | v1.2 | Current. The canonical protocol overview and changelog in `docs/protocol/`, the site pages in `content/` presenting STRATA Protocol v1.2, GitHub-readable stratum pages, and a regenerated public tree. Not a drop-in revision for a project derived against STRATA Protocol v1.0: see the upgrade path below |
| 1.1.0 | v1.1 | Retroactive release of v1.1, stamped 2026-08-06. Its protocol overview is a reconstruction; see that release's `docs/protocol/PROVENANCE.md`. Its site pages in `content/` still presented STRATA Protocol v1.0, as the site did when v1.1 was stamped |
| 1.0.0 | v1.0 | Initial public release, 2026-06-26 |

What changed at each version, the version DOI of each release, and an upgrade path for projects derived against STRATA Protocol v1.0 are on the [Protocol Version History](https://strataprotocol.org/changelog/protocol-versions) page. The protocol's own record of each change is [`docs/protocol/CHANGELOG.md`](./docs/protocol/CHANGELOG.md).

## Author

STRATA Protocol was created by **[Florante Pascual](https://florantepascual.com)** — a software engineer and technology thought leader with over a decade of experience building enterprise systems, AI-integrated platforms, and developer frameworks. See [`AUTHORS.md`](./AUTHORS.md).

- Website: [florantepascual.com](https://florantepascual.com)
- LinkedIn: [linkedin.com/in/florantepascual](https://www.linkedin.com/in/florantepascual/)
- GitHub: [github.com/FlorantePascual](https://github.com/FlorantePascual)

## Citation

If you reference STRATA Protocol in academic or professional work, please cite this repository using the metadata in [`CITATION.cff`](./CITATION.cff), or use the formats on the [citation page](https://strataprotocol.org/documentation/citation).

[![DOI](https://zenodo.org/badge/1263059137.svg)](https://doi.org/10.5281/zenodo.20939882)

## License

The documentation content in this repository is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](./LICENSE).

You are free to share and adapt this material for any purpose, provided you give appropriate credit to Florante Pascual and link back to [strataprotocol.org](https://strataprotocol.org).

## Issues & Community

For framework questions, documentation feedback, or community discussion, please [open an issue](https://github.com/FlorantePascual/strata-protocol-docs/issues).
