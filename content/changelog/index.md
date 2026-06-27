---
title: Changelog
description: STRATA Protocol version history — platform updates, methodology changes, and new feature releases.
status: stable
navigation:
  order: 0
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-06-06"
---

:status-badge{status="stable"}

The complete version history for StrataProtocol.org — every platform update, content release, and methodology change. Built entirely under the STRATA Protocol it documents, StrataProtocol.org progressed from bootstrap to a production-grade documentation portal — authentication, a PostgreSQL-backed deployment, member implementation templates, AI governance prompts, a legal framework, and a self-referential case-study trail — in five weeks across twenty-two governed requirements. For the current state of each section and what's planned next, see the [Platform Roadmap](/changelog/roadmap).

---

## 2026-06-27 — REQ-021: Application Version & Build Metadata

**The running application now reports its own version, commit, and build — the traceability the framework demands, applied to the platform itself.**

REQ-021 adds a single committed source of version and build identity, surfaced in the header badge, the footer, and a public Version & Build page — with zero new dependencies and no change to the build, server, or RBAC layers.

### What Was Delivered

- **Metadata foundation:** `package.json` declares `version: "1.0.0"` and a description; `scripts/generate-app-info.mjs` (plain Node ESM, no new dependency) stamps the committed `shared/generated/app-info.ts` from `package.json` plus best-effort Git (short commit SHA, branch) and a build timestamp — falling back to `"unknown"` and always exiting cleanly when no Git is present
- **Reproducible by design:** the generated module is committed and read verbatim by the Git-less Docker/Coolify build, so local and production builds report identical metadata; a drift-guard test fails CI if `package.json` and the generated version ever diverge
- **Typed access layer:** `shared/utils/app-info.ts` accessors and formatters (`getAppInfo`, `versionLabel`, `formatBuildDate`, `isKnown`, `resolveAppInfo`), with co-located tests for the formatters, the drift guard, and the public-route invariant
- **Site chrome:** the header badge changed from the hard-coded "Beta" to `v1.0.0` (success color); the footer gained a subtle build line (version · commit) linking to the version page
- **Version & Build page** (`/documentation/version-build`, public): every value is rendered through an `:app-version` MDC component — no version, commit, or date is hard-coded in prose
- **RBAC:** none — the version page is public by design; a spec invariant enforces that it stays out of `PROTECTED_ROUTES`

---

## 2026-06-27 — Maintenance: Live DOI Reconciliation, Deployment Hardening & Link Fixes

**Post-release corrections following the v1.0.0 Zenodo archive and the Case Studies launch.**

A cluster of small, cross-cutting fixes shipped after REQ-013: the citation layer was reconciled with the now-live Zenodo DOI, a containerized-build out-of-memory failure was resolved, and several internal and identity links were corrected.

### What Was Delivered

- **Live Zenodo DOI reconciliation:** the `/documentation/citation` page replaced its "DOI is being minted" placeholder with the issued concept DOI (`10.5281/zenodo.20939882`) and noted the v1.0.0 version DOI (`10.5281/zenodo.20939883`); the APA and BibTeX strings gained the Zenodo publisher and `doi.org` resolver URLs
- **Author identity expanded:** the shared author record gained ORCID and Zenodo DOI fields, now emitted in the Person and TechArticle JSON-LD `sameAs` arrays so scholarly identity links resolve sitewide
- **Deployment OOM fix:** the Docker build step runs under `NODE_OPTIONS=--max-old-space-size=4096`, preventing the Nuxt build from being out-of-memory-killed in the container
- **Case #1 link fix:** every internal link in the StrataProtocol.org case study was converted to an absolute path, closing a relative-resolution 404 class on index routes (`./artifact-trail` had resolved to `/case-studies/artifact-trail` instead of the nested target)
- **Identity link refresh:** the marketplace author card's portfolio and discovery-call links updated to the current `florantepascual.com` destinations

---

## 2026-06-25 — REQ-013: Self-Referential Artifact Trail

**The platform now proves its own thesis — the Case Studies section is live.**

The Case Studies section was migrated from flat placeholders to the nested proof structure and authored end to end. StrataProtocol.org documents its own construction as a public, navigable self-referential proof, and three pre-STRATA projects are published as a tiered proof-asset ladder — all under the RBAC Integrity Rule.

### What Was Delivered

- **Section migration:** four flat `early-access` placeholders converted to nested `{case}/` folders (CFS-001); the landing reframed from a single "Class 1.3" claim to a varied-class evidence framing — self-referential proof, formal early adoption, and diverse stacks
- **Case #1 (StrataProtocol.org — public, Class 1.3):** a self-referential overview plus artifact-trail and templates index pages that link directly to the platform's own governed records in the public `strata-protocol-docs` repository — a reader learning *about* an artifact trail can inspect the real one that produced the page
- **Cases #2–4 public summaries** (Fractal OSI — Class 1.2; One Verse Daily — Class 1.1; AI9GM — Class 1.3): project overview, technology stack, retrospective classification, "STRATA principles observed," and "what STRATA would have formalized," each closing with a tiered call to action
- **Proof-asset ladder (REQ-014):** each third-party case adds a `customer`-gated full five-stratum study and a `partner`-gated development journal — six new gated pages distilled-vs-expanded from the same source briefs as the summaries
- **RBAC:** six new `PROTECTED_ROUTES` prefixes (`…/full` → customer, `…/journal` → partner) wired under the four-step Integrity Rule; the entire StrataProtocol.org case stays public (BR-RULE-08); `content-guard.ts` untouched; 15 new spec assertions (227 total)
- **Mobile navigation fix:** the mobile slideover menu now honors the pedagogical `navigation.order` instead of rendering alphabetically, matching the desktop tabs

---

## 2026-06-24 — REQ-011: Documentation Platform Quality

**Platform quality locked as CI-enforced invariants; mobile rendering polished.**

REQ-011 verified the platform's foundational quality bar, converted the most critical operating rules into executable test-enforced invariants, reconciled PRD drift, and resolved two mobile cosmetic defects.

### What Was Delivered

- **Executable platform invariants** (`shared/utils/platform-invariants.ts`): six pure predicates locking SSR-only delivery (no `nuxt generate`), the Docus-bundled-dependencies rule, the Windows `hooks.ready` CSS fix, and the midnight theme as CI-enforced properties; 29 co-located tests, each naming the violated rule on failure
- **Functional verification recorded:** visual sign-off across the running build, type-check clean, full unit suite green; Lighthouse audit (home/docs LCP ≤ 0.6s, CLS 0) and the zero-external-font posture confirmed
- **PRD drift reconciled:** override paths, GitHub URL, footer and auth-component references, and navigation order corrected; a `folder-structure.md` repo-layer guide added
- **Quality-bar handoff** recorded for the REQ-014 consulting surfaces (`/engage`, `/apply`, membership display)
- **Mobile rendering fixes:** the STRATA acronym strip no longer clips on phones (clamp-based font/padding scaling); the header wordmark logo's aspect ratio is restored (Tailwind preflight `max-width` cap cancelled)

---

## 2026-06-17 — REQ-020: Legal Framework — Policies, Clickwrap & Footer

**Five legal policies, clickwrap consent, and a footer legal row shipped — zero new dependencies.**

A Docus-native legal layer was added: five public policy pages, a reusable in-app legal modal, clickwrap consent on registration and login, and a footer legal-links row — all driven from a single configuration source.

### What Was Delivered

- **Legal config foundation** (`shared/utils/legal.ts`): a `LEGAL_DOCS` registry (single source, reusing the operator identity) and a `:legal-meta{name="..."}` component that renders every legal fact — no hard-coded prose; a public-by-design invariant test asserts no `/legal/*` slug is ever in `PROTECTED_ROUTES`
- **Five public policy pages:** Terms of Use, Privacy Policy, Cookie Policy, Acceptable Use Policy, and a forward-looking Refund & Billing Policy; the Privacy Policy lists only actually-collected data and explicitly disclaims cloud credentials, files, and tracking
- **Reusable legal modal** (`useLegalModal` + `LegalModal.vue`): reuses `queryCollection` + `ContentRenderer` (no `marked`/`DOMPurify`/`v-html`); in-content `/legal/*` links swap the document in place
- **Clickwrap consent** sentence mounted beneath the submit button on the registration and login forms
- **Footer legal row:** a `<nav aria-label="Legal">` row of links to all five pages, ordered from the single config (Refund & Billing last)

---

## 2026-06-15 — REQ-019: Recognition & Citation

**The framework is now citable — an in-site citation page and discoverability links.**

REQ-019 establishes a citation layer over the existing ecosystem: a public citation page with formatted reference strings and discoverability entry points, ahead of a pending Zenodo DOI.

### What Was Delivered

- **Public citation page** (`/documentation/citation`): APA and BibTeX reference strings at v1.0.0, in a DOI-pending two-pass state; flat and navigation-excluded
- **Discoverability:** a "Cite the Framework" link added to the documentation TOC, and a citation link added to the existing `/about` call to action
- **No schema, RBAC, or code-logic change;** the footer citation link is documented but left disabled pending the DOI

---

## 2026-06-13 — REQ-018: Fractal Canvas Embed Component

**Interactive STRATA diagrams are now embeddable in content via `::fractal-canvas`.**

A configurable iframe-based embed component was added to render the STRATA fractal-diagram app inside content pages, with strategic illustrative embeds placed across the documentation.

### What Was Delivered

- **Embed component:** `shared/utils/fractal-embed.ts` pure URL builder (33 tests) and `app/components/content/FractalCanvas.vue`, a four-state iframe wrapper (skeleton overlay, deferred src, scroll-source identity); `useIframeWheelHandoff` + `iframe-scroll.ts` (23 tests)
- **Runtime config:** `appEmbedBaseUrl` (`NUXT_PUBLIC_APP_EMBED_BASE_URL`), reserved for the future STRATA Protocol App
- **Strategic embeds:** placed on the homepage, The Five Strata, and three stratum deep-dives; three `.fractal` diagram models authored (derivation chain, authority chain, enriched protocol model)
- **Authoring guide:** `developer-guide/reference/` converted to a folder with a customer-gated `embed-canvas.md` guide (RBAC wired)
- **Mobile follow-up (2026-06-24):** the embed height is capped to a viewport fraction on phones so it no longer intercepts and traps page scrolling

---

## 2026-06-09 — REQ-012: PostgreSQL Migration & Docker Deployment

**The database pivoted SQLite → PostgreSQL, and a production Docker stack shipped.**

The persistence layer was migrated from `better-sqlite3` to PostgreSQL (a separate container, stateless app) and every Drizzle call site refactored from synchronous to asynchronous, enabling a containerized deployment.

### What Was Delivered

- **Dependency and config swap:** `better-sqlite3` replaced with `pg` (a pure-JS driver, no native bindings); Drizzle dialect `sqlite` → `postgresql`
- **Schema translation:** `sqlite-core` → `pg-core` across six tables (timestamp and boolean column types converted); migrations regenerated
- **Connection layer rewrite:** `pg.Pool` + node-postgres with a pool-drain shutdown hook and a bounded database-readiness retry; a Drizzle PG migrator + async admin seed replacing the bespoke SQLite runner
- **Async call-site refactor:** all 24 Drizzle call sites converted from `.get/.all/.run` to `await` across the auth, user, and admin endpoints and the content guard — zero logic change
- **Docker artifacts:** an Alpine application image plus a PostgreSQL compose stack (database port internal-only), verified healthy end to end
- **Hardening (Phase 7):** a production-SSR routing fix for `/about`; relocation of the Nuxt Content database; and a closed access-control gap on `/ai-playbook/concepts` (frontmatter + `PROTECTED_ROUTES` + tests)

---

## 2026-06-08 — REQ-017: Authorship & SEO Identity

**Verifiable authorship and structured SEO across every page.**

A single author/identity source now drives footer attribution, an identity hub, and per-page structured data — establishing crawlable authorship and Open Graph metadata sitewide.

### What Was Delivered

- **Identity foundation:** `shared/utils/author.ts` (`AUTHOR` + `SITE` typed constants with field/URL tests); the `content.config.ts` docs schema extended with optional `authors` and `date`
- **Global SEO plugin:** WebSite JSON-LD plus `author`, `article:author`, and `og:site_name` metadata injected on every page
- **Per-page SEO:** a full `app/app.vue` Docus-app override + `useSeoHead` composable injecting TechArticle JSON-LD on documentation pages, per-route Open Graph tags, and a canonical link
- **Footer authorship statement:** the author name as a crawlable link, with a dynamically computed copyright year
- **Frontmatter migration:** `authors` and `date` added to all 53 content files
- **Identity Hub:** a public `/about` page with Person JSON-LD, an "About the Author" TOC link, and public GitHub attribution

---

## 2026-06-05 — REQ-014: Qualified Engagement — Consulting Model

**Subscription billing was replaced with a qualified-access consulting model.**

The platform pivoted from self-serve Stripe subscriptions to a discovery-call qualification model: member access is granted through a consulting conversation rather than a paywall.

### What Was Delivered

- **Engage section:** `content/updates/` replaced by a public `content/engage/` Services & Engagement page; `/updates` absorbed into `/changelog/updates`
- **Access-denied reframe:** the tier-gate surface rewritten as a welcoming qualified-access invitation (book a free discovery call) rather than an upgrade wall
- **Homepage redesign:** a tightened hero, a new Engage section, and a global section-spacing reduction
- **Qualification wizard:** a six-step public `/apply` flow with a Calendly popup, backed by `server/api/engage/qualify.post.ts` (rate-limited, allow-list-validated) and an operator email notification
- **Billing removal:** `server/api/billing/` and the Stripe client deleted; the `stripe` dependency uninstalled; the login-time grace-period demotion removed; the subscription page reduced to a membership display; a full messaging-compliance audit

---

## 2026-06-04 — REQ-016: Changelog Currency Update

**The changelog was backfilled and date-corrected into a complete version history.**

The changelog was assembled into a complete record: eight missing requirement entries added, three dates corrected against the commit log, and the build-velocity narrative introduced.

### What Was Delivered

- **Eight missing entries** added (REQ-001, REQ-003, REQ-004, REQ-005D, REQ-006, REQ-007, REQ-008, REQ-015) in newest-first order; the REQ-005C entry expanded with its Phase 3–8 deliverables
- **Three dates corrected** against the running commit log (REQ-000, REQ-005, REQ-005C)
- **Baseline entry:** a separate 2026-05-20 Initial Commit entry added; the introduction's build-velocity narrative introduced

---

## 2026-06-04 — REQ-008: Authentication, Membership & Authorization

**Production-grade authentication and subscription system live.**

StrataProtocol.org's full membership and billing infrastructure replaces the initial MVP environment-variable user store with a production-grade SQLite database, self-service registration, password reset, and Stripe-backed subscription tiers. All seven implementation phases were manually verified in a running browser environment with 88 unit tests passing.

### What Was Delivered

- **Database foundation:** Drizzle ORM + SQLite replaces MVP env-var user store; custom migration runner with idempotency tracking; dual user model (`membershipLevel: community | customer | partner` + `systemRole: user | admin`); `effectiveRole()` + `canAccess()` as the single RBAC decision point across all guards
- **Registration:** self-service account creation with bcrypt cost-12 hashing; 6-digit email verification code (15-minute expiry, 5-attempt invalidation); atomic rollback if email delivery fails
- **Email infrastructure:** dual-provider service (Postmark primary, Mailgun failover); shared rate-limiter factory reused across six auth endpoints
- **Password reset:** 32-byte URL token (256-bit entropy); `passwordChangedAt` session invalidation — prior sessions rejected at the Content API layer after a password change; `loginAt` stored in sealed session to enable the check
- **Profile management:** `/account/profile` with inline name edit and `fetchSession()` re-sync; `/account/change-password` with live five-rule strength indicator and forced-rotation flow for the seeded admin
- **Stripe subscription integration:** Checkout Session creation; webhook with idempotency via PRIMARY KEY constraint on processed events; cancel-at-period-end; Customer Portal; grace period demotion enforced at login; Stripe SDK v22 breaking changes handled via helper functions
- **Admin user management:** nine API endpoints covering search, view, edit, disable, enable, force-reset, membership override, role grant, and revoke; self-lockout prevention; full actor+target audit event recording
- **Security hardening:** login rate limiting (10 attempts / 15 min / IP, checked before bcrypt); access-denied page with contextual messaging and upgrade CTAs replacing raw Nuxt 403 errors
- **Audit log UI:** `/admin/audit-log` with filters for event type, actor/target user ID, and date range; 22+ event types color-coded
- **SQLite backup script** (`scripts/backup-db.sh`): WAL-safe backup using `sqlite3 .backup`; post-backup integrity check; three cloud storage providers (R2/S3/B2); non-zero exit on failure for alerting integration

---

## 2026-06-03 — REQ-007: AI Playbook — Development Prompts

**Thirteen AI governance prompts published — a complete prompt set for each STRATA stratum.**

The AI Playbook section was restructured from a flat placeholder into a multi-page member library, and thirteen self-contained, tool-agnostic prompts were authored against each stratum's authoritative deep-dive documentation. All prompts embed the full STRATA framework context so practitioners can paste them into any AI tool without additional setup.

### What Was Delivered

- **Section restructure:** flat `guides.md` stub converted to `guides/` multi-page folder per CFS-001 Rule 2; navigation order corrected to Introduction (1) → Concepts (2) → Guides (3)
- **`content/ai-playbook/introduction.md`** (public): governance vs. prompt engineering distinction, three AI roles (Responder / Generator / Documenter), authority chain constraint mechanism at session level
- **`content/ai-playbook/concepts.md`** (public): five-node authority chain pattern, session mandate three-question constraint model, session drift failure mode and why it is invisible in output
- **`content/ai-playbook/guides/index.md`** (customer-gated): four-step prompt usage workflow, five stratum navigation cards
- **Stratum 1 — Classification (2 prompts):** Classification Analysis (embeds full Class × Category framework, 7 challenge questions, Classification Record template); Classification Record Review (four-section structured critique)
- **Stratum 2 — Derivation (3 prompts):** Vision & Business Case Derivation, BRD Derivation, PRD Derivation — each with sequential gate conditions and labeled input blocks for upstream documents
- **Stratum 3 — Authority Chain (3 prompts):** Chain Setup (five-node chain, four completeness questions, full Copilot Instructions template); Session Mandate (nine bracket placeholders, strictly project-agnostic); Chain Review (gap analysis)
- **Stratum 4 — Execution Loop (3 prompts):** Phase Initiation brief; Phase Gate Review with PASS/RETURN/HOLD verdicts; Deviation Recording aligned to canonical DEVIATION notation from Session Mandate
- **Stratum 5 — Artifact Trail (2 prompts):** Artifact Trail Audit (six categories, Stage 2 governing completeness question); Conditioning (two IP filters, seven transformation steps, MDC authoring conventions)

---

## 2026-06-02 — REQ-015: Header Polish — Beta Badge & Documentation Navigation

**Header polish shipped — Beta badge, universal auth controls, and Docs entry point on every page.**

Three UX defects in the site header were resolved in a single session: the platform had no visible beta-state signal, the homepage had no header-level path to documentation, and authentication controls were inaccessible from non-documentation pages.

### What Was Delivered

- **`AppHeaderLeft.vue`** override: persistent "Beta" `UBadge` (size xs, variant subtle, color primary/violet) immediately adjacent to the STRATA logo, visible on every page including homepage and login
- **`AppHeaderCTA.vue`** override: always-rendered right-slot component with conditional "Docs" icon-button (hidden on documentation pages where section tabs are already present), plus sign-in/sign-out auth controls accessible from every page
- **`AppHeaderBottomRight.vue`** reduced to an empty `<div />` structural placeholder — auth controls migrated to the universal CTA slot
- Three UX defects resolved: absence of beta-state signal; dead-end homepage navigation with no header-level path to `/documentation`; auth controls inaccessible from the homepage and login page

---

## 2026-06-02 — REQ-006: Implementation Templates for Members

**Three-stack feature template library published — copy-pasteable STRATA governance artifacts for members.**

The Developer Guide section was restructured with a public monetization funnel (landing + introduction pages) while all template content remains member-gated. Three complete, production-quality feature templates were authored across the Nuxt/Docus, NestJS Backend, and Vue Frontend stacks, each with worked examples drawn from real STRATA-governed projects.

### What Was Delivered

- **RBAC split:** `/developer-guide` catch-all protection replaced with four granular sub-section prefixes, enabling public funnel pages (landing + introduction) per REQ-014 monetization model while keeping all template content member-gated; 7 new spec test cases (20/20 total)
- **`content/developer-guide/introduction.md`** (public): three-phase STRATA governance process, stack selection guide, strata-to-template mapping, tiered CTA
- **`content/developer-guide/getting-started.md`** (customer-gated): five-step quickstart workflow, stack comparison table, navigation cards to all three template sets
- **Nuxt/Docus Feature Template** (`guides/nuxt-docus/feature-template.md`, 757 lines): 9-section template covering full-stack structural analysis (6 sub-sections), RBAC considerations, data model, API contract, security, component and composable specs, and phased implementation; REQ-006 itself as primary worked example
- **NestJS Backend Feature Template** (`guides/nestjs-backend/feature-template.md`, ~5,000 words): 8-section template with TypeORM entity, migration SQL, five-endpoint API contract with full DTOs, three-layer test specification; FR-007 Team Task API as hypothetical worked example
- **Vue Frontend Feature Template** (`guides/vue-frontend/feature-template.md`, ~5,100 words): 8-section template with component specifications, composable specifications, Vitest+@vue/test-utils patterns; FR-012 Task Board UI as worked example building on the NestJS FR-007 backend
- **`content/developer-guide/architecture.md`:** template design rationale, conditioning pipeline, versioning model, cross-template continuity documentation
- **`content/developer-guide/reference.md`:** 25-entry section quick-reference table across all three stacks, AI prompts placeholder linking to `/ai-playbook/guides`

---

## 2026-06-01 — REQ-005D: Content File Structure Standard (CFS-001)

**Content File Structure Standard (CFS-001) ratified and verified across the full content tree.**

CFS-001 formalizes the structural rules governing the `content/` Markdown tree and establishes pedagogical ordering as a normative requirement. After ratification by human engineer review on 2026-05-31, the full `content/` directory was audited and all identified defects remediated before CFS-001 status advanced from normative to active.

### What Was Delivered

- **CFS-001 formally adopted** as the normative content authoring governance artifact for StrataProtocol.org
- **Four governance files updated:** Rule 4 (no redundant body H1) and pedagogical ordering policy added to both operative constitutions (`CLAUDE.md`, `.github/copilot-instructions.md`) and both content rule files
- **D3 remediation:** four section `index.md` files corrected from `navigation.order: 1` to `0` — eliminates indeterminate sidebar ordering when a section overview ties with its first leaf sibling (`ai-playbook`, `case-studies`, `developer-guide`, `documentation`)
- **D4 remediation:** thirteen pages scrubbed of duplicate body `# H1` headings that were producing doubled visible headings in the Docus rendered layout
- **R-008 typo fix:** `copilot-instructions.md` British-spelling forbidden-column corrected
- **Full-tree compliance verification:** all four CFS-001 rules confirmed active across the entire `content/` directory; CFS-001 status advanced from `normative` to `active`

---

## 2026-05-30 — REQ-005C: WIP Messaging & Progressive Launch

**StrataProtocol.org is now public-launch ready.**

Every section of the platform has been converted from a bare stub into an intentional, value-bearing work-in-progress placeholder. The platform now presents as *a living and expanding ecosystem* — not a partially incomplete site.

### What Was Added

- `StatusBadge` component: a status pill (`Stable`, `Preview`, `Early Access`, `Expanding`, `In Progress`, `Planned`) that appears on every section landing page.
- Platform Roadmap page (`/changelog/roadmap`): the canonical view of what is live, in progress, and planned.
- `status`, `tags`, `category`, `difficulty`, `version`, and `lastUpdated` metadata fields added to the content schema.
- All placeholder section pages rewritten with: intro, Available Today resources, Roadmap direction, cross-links, and member-gating CTAs where relevant.
- `PartnerCard.vue` MDC component: stateless partner card with YAML-block prop arrays, accessible `<article>` landmark, external links with `target="_blank" rel="noopener"`, ARIA annotations.
- `content/marketplace/index.md` rewritten: four partner categories (Implementation Partners, Advisory Services, Templates, Developer Tools), seed `PartnerCard` seeded with real platform author.
- `content/case-studies/index.md` rewritten: use-case preview grid for four projects, all correctly identified as Class 1.3 applying the full five-stratum sequence; proto-STRATA evolution narrative ("STRATA was extracted, not invented").
- Four case study stubs upgraded with status badges, tech stacks, correct five-stratum framing, and back-links.
- British-to-American English corrections across five content files (20 corrections); American English writing directive added to all four operative constitution files.
- Three new adaptive SVG diagrams: `devguide_architecture.svg` (four-layer platform architecture), `devguide_workflow.svg` (five-stage governed delivery workflow), `devguide_implementation_flow.svg` (six-node implementation flow with test lane).
- `content/documentation/developer-toolkit.md` — new public conversion page embedding three diagrams with member resource catalog and CTA.
- `content/developer-guide/index.md` upgraded from stub to structured member landing.
- `content/ai-playbook/index.md` upgraded from stub to WIP placeholder with stratum-specific guide previews.
- `content/blog/` renamed to `content/updates/` — section URL migrated from `/blog` to `/updates`; display title updated to "Updates".
- Navigation and status consistency audit: AI Playbook Roadmap label corrected, Developer Toolkit promoted to Available Now.
- `PartnerCard.vue` accessibility fix: `sr-only` "(opens in new tab)" text added to all external links per WCAG 2.1 SC 3.2.2.

---

## 2026-05-27 — REQ-005B: Strata SVG Diagrams, Light Mode & Font Fix

**Strata diagrams shipped. Visual polish complete.**

### What Was Added and Fixed

- Five adaptive single-file SVGs for each STRATA stratum diagram — light and dark mode via `@media (prefers-color-scheme)`.
- `StrataDiagram` component for rendering adaptive SVGs in Markdown content.
- Light-mode contrast issues across the platform resolved.
- Font-stack and heading-weight issues fixed.

---

## 2026-05-25 — REQ-005: Member Deep-Dive Documentation

**All five stratum deep-dives published.**

Complete procedural documentation for every stratum is now available to members:

- Stratum 1 — Classification: How to classify correctly using the seven challenge questions.
- Stratum 2 — Derivation: Full document hierarchy walkthrough with worked examples.
- Stratum 3 — Authority Chain: Constitutional mandate setup for AI coding tools.
- Stratum 4 — Execution Loop: Phase planning, AI delivery, and human gate protocol.
- Stratum 5 — Artifact Trail: Maintaining the living governance record.

---

## 2026-05-25 — REQ-004: Public Glossary

**Two-tier STRATA vocabulary reference published — 12 public terms and 42 member terms.**

The Documentation section's Reference sub-section was populated with two STRATA vocabulary resources: a free public glossary for orientation and a comprehensive member glossary covering the full practitioner vocabulary across all five strata.

### What Was Delivered

- **Public Glossary** (`content/documentation/reference/index.md`): 12 foundational STRATA terms across eight alphabetical sections, each with a practitioner-level definition and *Used in:* usage context
- **Full Member Glossary** (`content/documentation/reference/full-glossary.md`, customer-gated): 42 terms across 13 letter sections — a strict superset of the 12 public terms, expanded with per-stratum procedure vocabulary, template and session terminology, comparative definitions, and technical platform concepts
- `requiredRole: "customer"` protection on the Full Glossary confirmed intact at both frontmatter and `PROTECTED_ROUTES` layers

---

## 2026-05-24 — REQ-003: Getting-Started Guide

**Getting-Started guide published — from zero knowledge to a governed first step.**

The Getting-Started guide gives any public visitor a clear, sequential path into STRATA practice without requiring an account. All internal links resolve to public pages with no auth redirect risk.

### What Was Delivered

- Complete five-section practitioner onboarding guide at `content/documentation/getting-started.md` (~1,035 words)
- Section 3: five-stratum orientation using numbered steps with Phosphor icons matching the stratum narrative pages
- Section 4: three-step public workflow with a copy-pasteable artifact trail folder structure in a fenced code block
- Section 5: four Documentation hub navigation cards plus a member CTA linking to `/login`
- All internal links verified as public-accessible — no auth redirect risk for first-time visitors

---

## 2026-05-24 — REQ-001: Public Framework Documentation

**Five stratum narratives and the public Documentation section live.**

The Documentation section was populated with nine production-quality public pages covering the complete STRATA Protocol framework. A structural pre-flight corrected a Nuxt Content v3 navigation duplication issue that affected all eight portal sections before content authoring began.

### What Was Delivered

- **Navigation deduplication fix:** 28 single-item content folders converted to flat `.md` files; 5 `.navigation.yml` files added to multi-item sub-folders — root cause fix for duplicate sidebar entries across all sections
- **Documentation section landing page** (`content/documentation/index.md`): framing, stratum overview, member CTA
- **Introduction page** (`content/documentation/introduction.md`): governance gap, STRATA purpose, audience, self-referential proof
- **Five full stratum narrative pages** in `content/documentation/the-strata/`, each with a seven-section structure, MDC callouts, and member deep-dive CTAs: Classification, Derivation, Authority Chain, Execution Loop, Artifact Trail
- **Governing Principles page** (`content/documentation/concepts.md`): five structural axioms that make the five-stratum architecture coherent
- MDC callout syntax standardized across all authored pages (`::tip` / `::warning` shorthand; `::callout{type="..."}` syntax removed)

---

## 2026-05-21 — REQ-000: Platform Bootstrap & Pre-Flight Checklist

**Full-stack StrataProtocol.org platform built from a boilerplate scaffold in one governed session.**

The May 20 Nuxt/Docus scaffold was migrated, hardened, and extended into a production-ready documentation portal: four core configuration files replaced, STRATA branding applied, 51 content stubs created across eight sections, two-layer RBAC installed, auth system implemented, and architecture visualization published. REQ-000 is the foundation all twelve subsequent requirements build on.

### What Was Delivered

- **Repository migration:** five scaffold artifacts deleted; `package.json`, `nuxt.config.ts`, and `content.config.ts` replaced with spec-compliant versions; `docus`, `nuxt-auth-utils`, `bcryptjs`, `better-sqlite3`, `vitest`, and `vue-tsc` installed
- **TypeScript and testing infrastructure:** dual-pass `scripts/type-check.mjs` (separate app and server contexts); `tsconfig.typecheck.json`; `vitest.config.ts` with co-located `*.spec.ts` support
- **STRATA app shell:** `midnight` theme (violet/zinc); `useSubNavigation.ts` composable overriding Docus's default alphabetical sort; `AppHeaderBottom.vue` + `AppHeaderBottomRight.vue` auth controls; `AppFooterLeft.vue` copyright
- **51-file content scaffold:** 8 `.navigation.yml` files and 43 Markdown stubs across all eight sections; 18 pages with `requiredRole: "customer"` frontmatter; `content/index.md` replaced with the full STRATA homepage (hero, five-stratum feature grid, architecture iframe placeholder)
- **Two-layer RBAC:** `shared/utils/protected-routes.ts` with six route prefix entries (most-specific-first); `server/middleware/content-guard.ts` guarding `POST /__nuxt_content/*/query`; `app/middleware/auth-guard.global.ts` two-phase client guard; 15 unit tests passing
- **Auth API:** `server/api/auth/login.post.ts` (bcryptjs cost-12 + `setUserSession`); `server/api/auth/logout.post.ts` (`clearUserSession`); `app/pages/login.vue` with `fetchSession()` pre-navigation guard preventing infinite redirect
- **Architecture visualization:** `app/components/content/StrataVisual.vue` auto-sizing postMessage iframe; `public/strata-visual.html` cleaned from ~2,300 to 637 lines — 16 `@font-face` CDN references removed, all Claude AI chrome removed, brand colors aligned to Tailwind CSS v3 violet and zinc tokens

---

## 2026-05-20 — Initial Commit

**Bare Nuxt/Content/Docus scaffold — project repository initialized.**

The starting checkpoint: a default Nuxt 4 / `@nuxt/content` starter with boilerplate `app/app.vue`, `app/components/Alert.vue`, `app/components/Counter.vue`, and `content/about.md`. No STRATA governance artifacts, no authentication, no documentation structure. REQ-000 begins from this baseline.

---
