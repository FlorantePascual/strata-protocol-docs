---
title: "Version & Build"
description: "The version and build of the running StrataProtocol.org application, and the STRATA Protocol version it publishes."
navigation:
  exclude: true
---

This page reports the exact version and build of the StrataProtocol.org application currently running, and the version of the STRATA Protocol it publishes. The two are separate versions, even when their numbers match: the application version tracks changes to this site, and the protocol release tracks published versions of the framework — part of practicing the traceability the STRATA Protocol framework defines.

## Application

- **Application:** StrataProtocol.org
- **Package:** :app-version{name="name"}
- **Version:** :app-version{name="version"}

## Build

- **Commit:** :app-version{name="gitCommit"}
- **Branch:** :app-version{name="gitBranch"}
- **Generated:** :app-version{name="buildDate"}

## Protocol

- **Protocol:** STRATA Protocol :protocol-version{name="version"}
- **Stamped:** :protocol-version{name="stampDate"}
- **Release:** :protocol-version{name="release"}

Every protocol version and release, with what changed in each, is listed in the [Protocol Version History](/changelog/protocol-versions). For release notes, see the [Changelog](/changelog). To cite a specific release of the framework, see [Cite the STRATA Protocol](/documentation/citation).
