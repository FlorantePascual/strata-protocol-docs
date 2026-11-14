# Provenance of `docs/protocol/`

**Release 1.2.0 · STRATA Protocol v1.2**

This folder holds the STRATA Protocol itself, as published in this release. Each
file below states where it comes from and how it is kept in step.

| Path | What it is | How it is produced |
|---|---|---|
| [`README.md`](./README.md) | The protocol overview: the five strata and the principles that govern them | A canonical copy of the STRATA Protocol v1.2 overview, stamped 2026-08-31, byte-identical to the protocol's source text |
| [`CHANGELOG.md`](./CHANGELOG.md) | The protocol's version history: what changed at each version, and what a project derived against an earlier version has to do about it | A canonical copy, byte-identical to the protocol's source text |
| [`change-history/`](./change-history/) | The findings registers behind each version | Register 03, which produced STRATA Protocol v1.2, is a canonical copy. Registers 01 and 02 are withheld; see [`change-history/README.md`](./change-history/README.md) |
| [`the-strata/`](./the-strata/) | The five stratum pages and their overview, readable on GitHub | Generated from the public pages of [strataprotocol.org/documentation/the-strata](https://strataprotocol.org/documentation/the-strata) when this release was staged. Each page names its source URL and the release it was rendered at |
| `PROVENANCE.md` | This file | Written for each release |

**The canonical copies are never edited here.** A correction to the protocol text
arrives from upstream as a new protocol version, published as a new release.

**The rendered pages follow the site.** Interactive models on the site become
links, and the diagrams are loaded from strataprotocol.org rather than copied, so
they always show what the site serves. Where a rendered page and the site
differ, the site is current.

**The stratum source texts are not published.** The protocol's full stratum
documents are member material. The pages in `the-strata/` are the public form of
each stratum, as the site presents it.

## Earlier releases

Release 1.1.0 published STRATA Protocol v1.1 retroactively. Its overview was a
reconstruction: the author's working copy of v1.1 predated three amendments made
after its stamp date, and the final text was rebuilt from that copy, from the
STRATA Protocol v1.2 text that inherited the amendments, and from the published
list of changes between the two. That overview, with its own provenance note,
remains in release 1.1.0 (tag `v1.1.0`). This release replaces it with the
canonical STRATA Protocol v1.2 overview and does not carry the
reconstruction forward.

Release 1.0.0 published the site's pages and this project's governance
artifacts. It carried no protocol text.

Every release is archived on Zenodo under one concept DOI,
[10.5281/zenodo.20939882](https://doi.org/10.5281/zenodo.20939882), which always
resolves to the latest release. The full version history, with each release's
version DOI, is at
[strataprotocol.org/changelog/protocol-versions](https://strataprotocol.org/changelog/protocol-versions).
