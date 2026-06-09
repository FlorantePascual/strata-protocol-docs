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

The complete version history for StrataProtocol.org — every platform update, content release, and methodology change. Built entirely under the STRATA Protocol it documents, StrataProtocol.org progressed from bootstrap to a production-grade documentation portal — authentication, subscription billing, member templates, and AI governance prompts — in fifteen days across twelve governed requirements. For the current state of each section and what's planned next, see the [Platform Roadmap](/changelog/roadmap).

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
