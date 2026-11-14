# `docs/_templates` — Implementation Template Sets

The STRATA implementation templates turn the protocol's execution loop into repeatable scaffolding for
one technology stack at a time. Each set carries the same prompt pack, a parameters file holding that
stack's values, and the templates for the documents a Feature Release produces: the analysis, the phased
implementation plan, the session index, and the per-phase implementation records.

**This folder publishes overviews, not the templates.** The full sets are part of the member toolkit at
[strataprotocol.org](https://strataprotocol.org). Three sets have a public overview of their prompt pack
here; the other five are listed so the catalog is complete.

## The eight sets

| Set | Stack | Public overview |
|---|---|---|
| `NUXT` | Nuxt 4 / Docus full-stack: server-side rendering, Nuxt Content, Nitro | [`NUXT/prompts.md`](./NUXT/prompts.md) |
| `BE` | NestJS backend services | [`BE/prompts.md`](./BE/prompts.md) |
| `FE` | Vue 3 single-page applications | [`FE/prompts.md`](./FE/prompts.md) |
| `Astro` | Astro sites with Vue islands | — |
| `FE-Angular` | Angular single-page applications | — |
| `FE-React` | React single-page applications | — |
| `NextJS` | Next.js applications on the App Router | — |
| `n8n` | Features delivered as n8n workflows | — |

## How the sets are versioned

The template tree is versioned as a whole, with an integer and a change log. Every set has run Feature
Releases since template tree VERSION 5; template tree VERSION 6 refined the `NUXT` set's analysis and plan
templates. The prompt pack has the same shape in every set: what differs between stacks is mainly the
parameters file.

## Learn more

- How the templates are used: [strataprotocol.org/developer-guide](https://strataprotocol.org/developer-guide)
- The execution loop they drive: [strataprotocol.org/documentation/the-strata/stratum-4-execution-loop](https://strataprotocol.org/documentation/the-strata/stratum-4-execution-loop), or [`docs/protocol/the-strata/`](../protocol/the-strata/) in this repository
