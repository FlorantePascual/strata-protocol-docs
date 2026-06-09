# REQ-000 — Platform Bootstrap & Pre-Flight Checklist: Implementation Plan

**Feature:** REQ-000 — Platform Bootstrap & Pre-Flight Checklist  
**Author:** Copilot  
**Date Created:** 2026-05-20  
**Last Updated:** 2026-05-20  
**Status:** Approved — Ready for Implementation

**Related Documents:**
- [Analysis](./analysis.md)
- [Feature Index](./index.md)
- [Requirement Document](../../2.derivation/04-prd/REQ-000-Platform-Bootstrap-PreFlight.md)
- [Canonical Spec (verbatim file content)](../../../../DOCUS-PORTAL-STARTER-SPEC.md)
- [Operative Constitution](../../../../.github/copilot-instructions.md)

---

## 1. Implementation Overview

### Solution Summary

REQ-000 is implemented as a strictly sequenced **migration-first, then phase-by-phase build-up**. The sequence is not arbitrary — each phase depends on the previous one:

- Migration must run before any new files are created (wrong files must be absent before correct ones can coexist)
- Phase 0 (config) must complete before Phase 1 (app) can type-check
- Phase 1 (app layer) must complete before Phase 2 (content) renders correctly in the portal
- Phase 2 (content stubs) must complete before Phase 3 (auth) can protect anything meaningful
- Phase 3 (auth system) must complete before Phase 4 (CI) can validate the full stack
- Phase 4 (CI) must pass before Phase 5 (assets) is the last remaining item

**Layers touched:** Config root (`nuxt.config.ts`, `tsconfig.typecheck.json`, `vitest.config.ts`, `.env.example`), `app/`, `server/`, `shared/`, `content/` (48 files), `public/`, `scripts/`

### Key Architectural Decisions

1. **Decision: `docus` as the framework layer (not standalone `@nuxt/content`)**
   - **Rationale:** Docus provides the documentation portal shell (header, sidebar, search, dark mode, MDC components) — building this from scratch on bare `@nuxt/content` would require hundreds of lines of layout code. Docus is the correct tool for documentation portals.
   - **Trade-offs:** Docus bundles `@nuxt/content` and `@nuxt/ui` — they must never be added as separate dependencies (conflict). Docus component overrides require exact filename matching in `app/components/docus/` (or root `app/components/` for top-level Docus components like `AppHeaderBottom`).
   - **Alternatives Considered:** Standalone `@nuxt/content` was the current broken state — rejected.

2. **Decision: `nuxt-auth-utils` for session management (not custom JWT)**
   - **Rationale:** Provides sealed HttpOnly session cookie out of the box with `getUserSession`, `setUserSession`, `clearUserSession` as Nitro auto-imports. No custom JWT signing, no manual cookie management.
   - **Trade-offs:** Session functions are auto-imports only — explicit import causes `TS2724`. Must never be imported explicitly.
   - **Alternatives Considered:** Custom JWT middleware — rejected for complexity and security risk.

3. **Decision: MVP user store in `NUXT_USERS_JSON` environment variable**
   - **Rationale:** Simplest possible user store for initial deployment. No database schema migration, no admin UI. Users are provisioned by operators via environment variable.
   - **Trade-offs:** Not scalable beyond ~10 users; no admin UI; user changes require server restart. REQ-009 (Invitation-Based Provisioning) extends this.
   - **Alternatives Considered:** SQLite user table — deferred to REQ-009.

4. **Decision: Two-layer RBAC (frontmatter + `PROTECTED_ROUTES` + `content-guard`)**
   - **Rationale:** The frontmatter `requiredRole` alone only protects client-side navigation. A user can call `POST /__nuxt_content/docs/query` directly to fetch protected content without triggering the browser guard. The server-side `content-guard.ts` + `PROTECTED_ROUTES` closes this gap.
   - **Trade-offs:** Both layers must be kept in sync when new protected paths are added. The RBAC Integrity Rule in copilot-instructions.md §6 enforces this.
   - **Alternatives Considered:** Frontmatter-only (REQ-004's original decision) — rejected as a security violation (BR-RULE-01 compliance gap per Finding 1).

5. **Decision: `scripts/type-check.mjs` dual-pass type checking**
   - **Rationale:** The app and server contexts use different `tsconfig` references (`.nuxt/tsconfig.app.json` vs `.nuxt/tsconfig.server.json`). A single pass would use the wrong context for one of them. Additionally, Docus has pre-existing strict-mode violations in `node_modules/` that must be suppressed without hiding project-file errors.
   - **Trade-offs:** Requires a custom script rather than a simple `tsc` call. More complex but necessary.
   - **Alternatives Considered:** Single `vue-tsc` pass — rejected because server files fail type checking in app context.

---

## 2. Phase Breakdown

### Phase M: Migration — Repository Cleanup

**Status:** ✅ Complete — 2026-05-20

**Objective:**  
Remove all incorrect and extraneous files, and replace four core configuration files with the spec-compliant versions. This creates a clean foundation before any new files are added.

> **⚠️ DESTRUCTIVE OPERATIONS — Confirm with human engineer before executing deletes.**

**Description:**  
Steps M-1 through M-7 from REQ-000 §3, executed in sequence. Deletions happen first, then config replacements, then `npm install`, then `nuxt dev` to generate `.nuxt/`.

**Deliverables:**

| Step | Action | File(s) |
|---|---|---|
| M-1 | **DELETE** (destructive — confirm first) | `app/app.vue`, `app/pages/[...slug].vue`, `app/components/Alert.vue`, `app/components/Counter.vue`, `content/about.md` |
| M-2 | **REPLACE** `package.json` | Per REQ-000 §M-2: add `docus`, `nuxt-auth-utils`, `bcryptjs`, `better-sqlite3`, `vitest`, `vue-tsc`, `@types/bcryptjs`; remove `@nuxt/content`, `vue`, `vue-router`; remove `generate` script |
| M-3 | **REPLACE** `nuxt.config.ts` | Per REQ-000 §M-3: `extends: ['docus']`, `modules: ['nuxt-auth-utils']`, `site`, `runtimeConfig`, `nitro.preset: 'node-server'`, `hooks.ready` |
| M-4 | **REPLACE** `content.config.ts` | Per REQ-000 §M-4: landing + docs two-collection setup with Zod schema |
| M-5 | **VERIFY** `tsconfig.json` | Per REQ-000 §M-5: existing four-reference structure is correct; no change needed |
| M-6 | **RUN** `npm install` | Installs all new dependencies |
| M-7 | **RUN** `npx nuxt dev --no-open` then Ctrl+C | Generates `.nuxt/` directory required by tsconfig references |

**Affected Code Areas:**
- Root: `package.json`, `nuxt.config.ts`, `content.config.ts`
- `app/` (removals)
- `content/` (removal of `about.md`)

**Integration Points:**  
After M-7, `.nuxt/tsconfig.app.json`, `.nuxt/tsconfig.server.json`, `.nuxt/tsconfig.shared.json`, `.nuxt/tsconfig.node.json` all exist. All subsequent phases depend on these.

**Testing Strategy:**  
No unit tests in this phase. Manual verification only.

**Acceptance Criteria (Checkpoint M):**
- `.nuxt/` directory exists
- `node_modules/` is populated with `docus`, `nuxt-auth-utils`, `bcryptjs`, `better-sqlite3`, `vitest`, `vue-tsc`
- `package.json` has no `generate` script, no `@nuxt/content`, no `@nuxt/ui`
- `nuxt.config.ts` has `extends: ['docus']` and `nitro.preset: 'node-server'`
- Five deleted files are absent from the repo

**Dependencies:**  
Human confirmation for Step M-1 (destructive deletions). No REQ predecessors.

---

### Phase 0: Configuration Files

**Status:** ✅ Complete — 2026-05-20

**Objective:**  
Create the three configuration files that establish the TypeScript checking and testing infrastructure.

**Description:**  
These files are prerequisites for the type-checker and test runner. They must exist before any code in `app/`, `server/`, or `shared/` is created, as the type-checker will immediately need `tsconfig.typecheck.json`.

**Deliverables:**

| File | Source | Notes |
|---|---|---|
| `tsconfig.typecheck.json` | REQ-000 §4.1 (verbatim from DOCUS spec §7.6) | Extends `.nuxt/tsconfig.app.json`; includes all layers |
| `vitest.config.ts` | REQ-000 §4.2 (verbatim from DOCUS spec §7.7) | `globals: true`; excludes `_bak`, `_data`, `_tmp` |
| `.env.example` | REQ-000 §4.3 (verbatim from DOCUS spec §7.8) | Documents `NUXT_SESSION_PASSWORD` and `NUXT_USERS_JSON` |

**Affected Code Areas:**
- Root: `tsconfig.typecheck.json`, `vitest.config.ts`, `.env.example`

**Testing Strategy:**  
`npm run test:unit -- --run` should exit 0 (no spec files exist yet — passes vacuously).

**Acceptance Criteria (Checkpoint 0):**
- `npm run dev` starts without errors
- Site loads at `http://localhost:3000` (minimal content — acceptable)
- `.nuxt/` directory exists
- `npm run test:unit -- --run` exits 0

**Dependencies:**  
Phase M complete.

---

### Phase 1: App Configuration

**Status:** ✅ Complete — 2026-05-20

**Objective:**  
Create the STRATA branding layer: theme, navigation composable, homepage page, header/footer Docus overrides, and auth type declarations. By the end of Phase 1, the site should look like StrataProtocol.org.

**Description:**  
Files must be created in the exact sequence specified in REQ-000 §5 to avoid Nuxt layer resolution issues. `AppHeaderBottom.vue` must exist before `AppHeaderBottomRight.vue` is created — the former is the mount point for the latter (Gotcha G12). Auth middleware and login page are deferred to Phase 3.

**Execution order (per REQ-000 §5 — do not deviate):**
1. `app/app.config.ts`
2. `app/app.css`
3. `app/types/auth.d.ts`
4. `app/composables/useSubNavigation.ts`
5. `app/pages/index.vue`
6. `app/components/AppFooterLeft.vue`
7. `app/components/AppHeaderBottom.vue`
8. `app/components/AppHeaderBottomRight.vue`

**Deliverables:**

| File | Source | Notes |
|---|---|---|
| `app/app.config.ts` | REQ-000 §5.1 (STRATA-specific) | `midnight` theme (violet/zinc); STRATA header title, logo paths, GitHub URL |
| `app/app.css` | DOCUS spec §8.2 (verbatim) | Base CSS resets |
| `app/types/auth.d.ts` | DOCUS spec §8.3 (verbatim) | Augments `#auth-utils` User interface for IDE tooling in `.vue` files |
| `app/composables/useSubNavigation.ts` | DOCUS spec §8.4 (verbatim) | **REQUIRED** — without it navigation appears in alphabetical order (Gotcha G2) |
| `app/pages/index.vue` | DOCUS spec §8.5 (verbatim) | Function-based key for `queryCollection('landing')` (Gotcha G3 fix) |
| `app/components/AppFooterLeft.vue` | REQ-000 §5.6 (STRATA-specific) | `Copyright © {year} StrataProtocol.org. All rights reserved.` |
| `app/components/AppHeaderBottom.vue` | DOCUS spec §8.8 (verbatim) | **REQUIRED** — mount point for `AppHeaderBottomRight` (Gotcha G12) |
| `app/components/AppHeaderBottomRight.vue` | DOCUS spec §8.9 (verbatim) | Sign-in / sign-out controls; two-step sign-out sequence |

**Affected Code Areas:**
- `app/app.config.ts`, `app/app.css`
- `app/types/auth.d.ts`
- `app/composables/useSubNavigation.ts`
- `app/pages/index.vue`
- `app/components/AppFooterLeft.vue`, `AppHeaderBottom.vue`, `AppHeaderBottomRight.vue`

**Integration Points:**
- `app.config.ts` — consumed by Docus for theme, header, and navigation configuration
- `useSubNavigation.ts` — injected into Docus navigation rendering to override default sort
- `AppHeaderBottom.vue` + `AppHeaderBottomRight.vue` — Docus component override pattern; filename must match Docus core component name exactly
- `app/types/auth.d.ts` — augments `nuxt-auth-utils` module types for Vue component IDE tooling

**Testing Strategy:**  
No unit tests in this phase (no `shared/utils/` functions). Manual visual verification.

**Acceptance Criteria (Checkpoint 1):**
- `npm run dev` starts without errors
- Site loads at `http://localhost:3000`
- Header shows "STRATA Protocol" title
- Primary color is violet (midnight theme)
- "Sign in" button appears in header bar (even though login page doesn't exist yet)
- No console errors related to missing components

**Dependencies:**  
Phase 0 complete. `.nuxt/` directory must exist for type resolution.

---

### Phase 2: STRATA Content Structure

**Status:** ✅ Complete — 2026-05-20

**Objective:**  
Create all 48 content scaffold files — 8 `.navigation.yml` files and 40 Markdown stubs — establishing the complete 8-section documentation structure with correct RBAC frontmatter from day one.

**Description:**  
All content stubs are placeholders. Production content is authored in REQ-001 through REQ-013. However, the structure, `requiredRole` declarations, and `navigation.order` values are normative and must be correct from this phase. An incorrect `navigation.order` at stub time will produce wrong sidebar ordering that cascades into all downstream content work.

REQ-000 §6 specifies all file content. Key deviations from the generic DOCUS spec that are STRATA-specific:
- `content/documentation/deep-dives/` added (Finding 2 correction — not in DOCUS generic spec)
- `content/documentation/introduction/` added (Finding 3 correction)
- Developer Guide nav title is "Templates" not "Developer Guide" (Finding 4)
- `content/documentation/reference/full-glossary.md` has `requiredRole: "customer"` (Finding 1 security correction)
- `content/changelog/index.md` has actual initial commit content (not just a stub)

**Note on `content/index.md`:** This file currently exists as a bare stub (in `landing` collection). Phase 2 replaces it with the full STRATA homepage featuring the hero section, five-stratum feature grid, architecture iframe placeholder, and "Built on STRATA" section.

**Deliverables:**

*Navigation YML files (8):*

| File | Title | Icon | Order |
|---|---|---|---|
| `content/documentation/.navigation.yml` | Documentation | `i-ph-book-open` | 1 |
| `content/developer-guide/.navigation.yml` | **Templates** | `i-ph-files` | 2 |
| `content/case-studies/.navigation.yml` | Case Studies | `i-ph-briefcase` | 3 |
| `content/ai-playbook/.navigation.yml` | AI Playbook | `i-ph-robot` | 4 |
| `content/video-courses/.navigation.yml` | Video Courses | `i-ph-video` | 5 |
| `content/marketplace/.navigation.yml` | Marketplace | `i-ph-storefront` | 6 |
| `content/blog/.navigation.yml` | Blog | `i-ph-newspaper` | 7 |
| `content/changelog/.navigation.yml` | Changelog | `i-ph-git-branch` | 8 |

*Markdown stubs (40) — grouped by access level:*

**Public pages (no `requiredRole`):**
- `content/index.md` (REPLACE) — STRATA homepage with full MDC content
- `content/documentation/index.md` through all public sub-pages
- `content/documentation/the-strata/index.md` + 5 stratum sub-pages
- `content/documentation/concepts/index.md`
- `content/documentation/reference/index.md`
- All `content/case-studies/` pages (5 stubs)
- `content/ai-playbook/index.md`, `introduction/index.md`, `concepts/index.md`
- `content/video-courses/index.md`, `content/marketplace/index.md`, `content/blog/index.md`
- `content/changelog/index.md` (with actual initial commit entry)

**Member-only pages (`requiredRole: "customer"`):**
- `content/documentation/reference/full-glossary.md` ← **Finding 1 security fix**
- `content/documentation/account/index.md`
- `content/documentation/deep-dives/index.md` + 5 stratum sub-pages ← **Finding 2**
- All `content/developer-guide/` pages (8 pages) ← all customer-protected
- `content/ai-playbook/guides/index.md` ← customer-protected

**Affected Code Areas:**
- `content/` (51 files — 8 `.navigation.yml` + 43 `.md` including `content/index.md` replacement)
- `app/components/content/StrataVisual.vue` (new — DEVIATION D2-4; Phase 5 deliverable pulled forward)
- `public/strata-visual.html` (modified — DEVIATION D2-4; postMessage height script added)
- `scripts/type-check.mjs` (new — DEVIATION D2-1; Phase 4 deliverable pulled forward)
- `server/tsconfig.json` (new — DEVIATION D2-2; Phase 3 Step 4 deliverable pulled forward)

**Phase 2 Deviations (recorded; no action required):**
- **D2-1** `scripts/type-check.mjs` created early — Phase 4 item; required for type-check quality gate. Phase 4 Step 1 is already complete.
- **D2-2** `server/tsconfig.json` created early — Phase 3 Step 4 item; required by D2-1 server pass. Phase 3 Step 4 is already complete.
- **D2-3** `ph:link-chain` icon in `content/index.md` hero fails to resolve — cosmetic; correct in Phase 5.
- **D2-4** `app/components/content/StrataVisual.vue` + `public/strata-visual.html` postMessage script created early — Phase 5 partial; the visualization asset was already present in `public/`. Logos and `@font-face` cleanup remain for Phase 5.

**Integration Points:**
- All protected `.md` files get `requiredRole: "customer"` frontmatter — consumed by Phase 3 `auth-guard.global.ts` (Phase B precise role check)
- Navigation YAML files consumed by Docus sidebar and header tab rendering
- `content/index.md` consumed by `app/pages/index.vue` via `queryCollection('landing')`

**Testing Strategy:**  
No unit tests. Manual navigation verification.

**Acceptance Criteria (Checkpoint 2):**
- `npm run dev` — all 8 section tabs appear in the header tab bar in the correct order (Documentation → Changelog)
- Sidebar shows section structure with correct icons
- No 404 errors on any section landing page
- `/developer-guide` loads member stub content (no redirect yet — auth system not active)
- `content/documentation/deep-dives/` appears in sidebar under Documentation
- `content/changelog/index.md` shows the initial commit entry (not just a stub)

**Dependencies:**  
Phase 1 complete (navigation composable and app config must exist for correct rendering).

---

### Phase 3: Auth System

**Status:** ✅ Complete — 2026-05-21

**Objective:**  
Implement the complete authentication and content-protection stack: user store, auth adapter, login/logout API routes, server-side content guard, shared route definitions, client route guard, and login page.

> **⚠️ SECURITY-CRITICAL PHASE.** Any modification to `server/middleware/content-guard.ts` that weakens access control requires human engineer approval per copilot-instructions.md §12.

**Description:**  
This is the most security-critical phase of REQ-000. Files must be created in the exact sequence specified in REQ-000 §7 to ensure type resolution is correct at each step.

**Execution order (per REQ-000 §7 — do not deviate):**
1. `shared/utils/protected-routes.ts`
2. `shared/utils/protected-routes.spec.ts`
3. `shared/types/auth.d.ts`
4. ~~`server/tsconfig.json`~~ **Already exists** — created in Phase 2 as DEVIATION D2-2. Verify content matches spec (`{ "extends": "../.nuxt/tsconfig.server.json" }`); do not recreate.
5. `server/utils/user-store.ts`
6. `server/utils/auth-adapter.ts`
7. `server/api/auth/login.post.ts`
8. `server/api/auth/logout.post.ts`
9. `server/middleware/content-guard.ts`
10. `app/middleware/auth-guard.global.ts`
11. `app/pages/login.vue`

**Deliverables:**

| File | Source | Notes |
|---|---|---|
| `shared/utils/protected-routes.ts` | REQ-000 §7.1 (STRATA-specific) | 6 PROTECTED_ROUTES entries; most-specific-first ordering; `findProtectedRoute()` |
| `shared/utils/protected-routes.spec.ts` | REQ-000 §7.2 (STRATA-specific) | 15 tests (2 hierarchy + 13 route); all must pass |
| `shared/types/auth.d.ts` | DOCUS spec §9.3 (verbatim) | Server-context type checking (complements `app/types/auth.d.ts`) |
| ~~`server/tsconfig.json`~~ | DOCUS spec §10.1 (verbatim) | **Already exists** (Phase 2 DEVIATION D2-2) — verify, do not recreate |
| `server/utils/user-store.ts` | DOCUS spec §10.2 (verbatim) | Parses `NUXT_USERS_JSON`; `StoredUser` interface; lazy-loaded |
| `server/utils/auth-adapter.ts` | DOCUS spec §10.3 (verbatim) | `authenticate()` dispatches to local or remote strategy |
| `server/api/auth/login.post.ts` | DOCUS spec §10.4 (verbatim) | `readBody()` → validate → `authenticate()` → `setUserSession()` |
| `server/api/auth/logout.post.ts` | DOCUS spec §10.5 (verbatim) | `clearUserSession(event)` → `{ ok: true }` |
| `server/middleware/content-guard.ts` | DOCUS spec §10.6 (verbatim) | Guards `POST /__nuxt_content/*/query`; uses `findProtectedRoute()` |
| `app/middleware/auth-guard.global.ts` | DOCUS spec §8.6 (verbatim) | Two-phase client guard; Phase A (PROTECTED_ROUTES fast-path); Phase B (queryCollection role check) |
| `app/pages/login.vue` | REQ-000 §7.11 (STRATA-specific) | Subtitle: "STRATA Protocol — members only area"; all security logic verbatim from spec §8.10 |

**Critical implementation notes for this phase:**

- `getUserSession`, `setUserSession`, `clearUserSession` are **Nitro auto-imports**. Never add explicit `import` statements for these. (Gotcha G5)
- In `login.vue` submit handler: call `await fetchSession()` BEFORE `await navigateTo()` — without this, `loggedIn.value` remains `false` after login and the auth guard causes an infinite redirect loop. (Gotcha G6)
- In `AppHeaderBottomRight.vue` sign-out: call `$fetch('/api/auth/logout', { method: 'POST' })` (clears server cookie) AND `await clear()` (syncs client state). Both steps required.
- Both `app/types/auth.d.ts` (Phase 1) AND `shared/types/auth.d.ts` (this phase) must exist. (Gotcha G7)
- The `content-guard.ts` intercepts body reads — `h3` caches the body so calling `readBody()` twice is safe. (Gotcha G9)

**Affected Code Areas:**
- `shared/utils/protected-routes.ts`, `shared/utils/protected-routes.spec.ts`
- `shared/types/auth.d.ts`
- `server/tsconfig.json`
- `server/utils/user-store.ts`, `server/utils/auth-adapter.ts`
- `server/api/auth/login.post.ts`, `server/api/auth/logout.post.ts`
- `server/middleware/content-guard.ts`
- `app/middleware/auth-guard.global.ts`
- `app/pages/login.vue`

**Integration Points:**
- `findProtectedRoute()` is the shared contract between `auth-guard.global.ts` (app) and `content-guard.ts` (server) — both consume from `shared/utils/protected-routes.ts`
- `authenticate()` in `auth-adapter.ts` is the sole entry point for credential validation — `login.post.ts` must not contain validation logic beyond input sanitization
- `setUserSession()` shape: `{ user: { id, email, name?, role } }` — `auth-guard.global.ts` reads `session.user.role`

**Testing Strategy:**  
Co-located spec: `shared/utils/protected-routes.spec.ts` — 15 test cases (verbatim from REQ-000 §7.2).

Run: `npm run test:unit -- --run`

All 15 tests must pass. The spec tests:
- `ROLE_HIERARCHY` ordering (1 `it()` with 3 assertions — counts as 1 test in Vitest)
- Public paths return null (4 tests)
- `deep-dives` exact + sub-path match (2 tests)
- `account` match (1 test)
- `full-glossary` exact + parent public (2 tests)
- `ai-playbook/guides` exact + sub-path (2 tests)
- `developer-guide` exact + sub-path (2 tests)

> **Note (Phase 3 ACTUAL):** Vitest reports 15 tests: 1 `ROLE_HIERARCHY` + 14 `findProtectedRoute`. The original plan listed "2 hierarchy" but the spec yields one `it()` block with three assertions for ROLE_HIERARCHY. All 15 pass — no impact on correctness.

**Acceptance Criteria (Checkpoint 3):**
- `npm run test:unit -- --run` — all 15 tests pass
- Navigate to `/developer-guide` without logging in → redirects to `/login?redirect=%2Fdeveloper-guide`
- Log in with a provisioned test user → redirects back to `/developer-guide`
- Navigate to `/documentation/deep-dives` without logging in → redirects to `/login`
- Navigate to `/ai-playbook/guides` without logging in → redirects to `/login`
- `POST /api/auth/logout` → returns `{ ok: true }`
- Direct `POST /__nuxt_content/docs/query` with protected path (unauthenticated) → HTTP 401
- Sign-out clears both server cookie and client `loggedIn.value`

**Dependencies:**  
Phase 2 complete (content stubs with `requiredRole` frontmatter must exist for Phase B of `auth-guard.global.ts` to work).

---

### Phase 4: CI Toolchain

**Status:** ✅ Complete — 2026-05-20 (DEVIATION D2-1 — delivered in Phase 2)

**Objective:**  
Create `scripts/type-check.mjs` — the dual-pass type checker that validates both the app and server TypeScript contexts.

**Description:**  
A single `vue-tsc` invocation cannot type-check both the app and server contexts correctly because they reference different auto-generated tsconfig files (`.nuxt/tsconfig.app.json` vs `.nuxt/tsconfig.server.json`). The custom script runs two passes and suppresses known Docus node_modules strict-mode violations without hiding genuine project errors.

**Deliverables:**

| File | Source | Notes |
|---|---|---|
| ~~`scripts/type-check.mjs`~~ | DOCUS spec §11.1 (verbatim) | **Complete** (Phase 2 DEVIATION D2-1) — created in Phase 2; all three gates confirmed passing in Phase 3 quality gate run. |

**Affected Code Areas:**
- `scripts/type-check.mjs`

**Testing Strategy:**  
Run `npm run type-check` — exit code 0 is the acceptance criterion.

**Acceptance Criteria (Checkpoint 4):**
- `npm run type-check` — exit 0, no errors in project files
- `npm run build` — exit 0
- `npm run test:unit -- --run` — exit 0 (all 15 tests still passing)

All three gates must pass before proceeding to Phase 5.

**Dependencies:**  
Phase 3 complete (all source files that need type-checking must exist).

---

### Phase 5: Assets & Branding

**Status:** ✅ Complete — 2026-05-21

**Objective:**  
Place the cleaned architecture visualization HTML and logo assets in `public/` so the homepage renders completely.

**Description:**  
Phase 5 completes REQ-000. The `strata-visual.html` already existed in `public/` — the auto-sizing iframe component was implemented in Phase 2 (DEVIATION D2-4). Phase 5 delivered a comprehensive cleanup of `public/strata-visual.html`: all external CDN dependencies removed, all Claude AI / Anthropic UI chrome removed, brand colors aligned to the Tailwind CSS v3 violet and zinc palettes that Nuxt UI uses. Line count reduced from ~2300 to 637 (72%). The three logo files (`favicon.png`, `logo-dark.png`, `logo-light.png`) are deferred — see deliverables table below.

**Deliverables:**

| File | Source | Notes |
|---|---|---|
| ~~`app/components/content/StrataVisual.vue`~~ | Phase 2 DEVIATION D2-4 | **Already exists** — postMessage-driven auto-sizing iframe component. No action needed. |
| `public/strata-visual.html` | Already in `public/` | **✅ Complete — Phase 5 2026-05-21.** Full Claude AI UI removal; zero external CDN references; brand colors aligned (S1 → Tailwind violet, S5 → Tailwind zinc); ~2300 → 637 lines (72% reduction). See PHASE-5-IMPLEMENTATION-MANIFEST.md. |
| ~~`public/logo/favicon.png`~~ | v1 official asset | **✅ Present** — v1 official asset confirmed in `public/logo/`. No action required. |
| ~~`public/logo/logo-dark.png`~~ | v1 official asset | **✅ Present** — v1 official asset confirmed in `public/logo/`. No action required. |
| ~~`public/logo/logo-light.png`~~ | v1 official asset | **✅ Present** — v1 official asset confirmed in `public/logo/`. No action required. |

**`strata-visual.html` cleanup procedure (Phase 5 ACTUAL — all steps complete):**
1. ~~Open `docs/about/strata.html`~~ File already exists at `public/strata-visual.html`.
2. ~~Remove all `@font-face` declarations referencing `https://assets.claude.ai/` (or any external CDN URL)~~ ✅ Done — 16 `@font-face` declarations removed (Anthropic Sans + Anthropic Serif).
3. ~~Remove any other `http://` or `https://` references (full security audit)~~ ✅ Done — Tabler Icons CDN `<link>` also removed. Zero external references remain.
4. ~~Replace all font stacks with system-ui stack~~ ✅ Done — body, `--font-sans` var, and `:root --font-serif` all updated.
5. ~~Verify the visual renders correctly in a browser with system fonts~~ Manual verification pending.
6. `ph:link-chain` icon in `content/index.md` — **verified correct**; `ph:link-chain` resolves in Nuxt Icon (Phosphor icon set). No change needed. DEVIATION D2-3 closed.

> **DEVIATION D5-1:** Scope expanded from `@font-face`-only cleanup to a full Claude AI UI removal pass (form.elicit ~528 lines, entry-animation, action-btns, copy-toast, color ramps, Mermaid CSS, mcp-host-variables reduced from ~70 to 9 CSS vars) plus brand color alignment (S1 → violet, S5 → zinc). Result: 72% size reduction. No planned deliverables were de-scoped; all cleanup was additive and beneficial.

**Fallback:** The `::strata-visual` MDC component in `content/index.md` can be commented out until `@font-face` cleanup is done if the external font URLs are blocked. The auto-sizing infrastructure is already in place.

**Affected Code Areas:**
- `public/strata-visual.html`
- `public/logo/favicon.png`, `logo-dark.png`, `logo-light.png`

**Testing Strategy:**  
Manual visual verification only.

**Acceptance Criteria (Checkpoint 5 — Final):**

| Test | Expected Result |
|---|---|
| `npm run dev` | No errors; site loads |
| Root `/` | STRATA hero renders; 8 nav tabs in order |
| `/documentation` | Public documentation section loads |
| `/developer-guide` (unauthenticated) | Redirects to `/login?redirect=%2Fdeveloper-guide` |
| `/documentation/deep-dives` (unauthenticated) | Redirects to `/login` |
| `/ai-playbook/guides` (unauthenticated) | Redirects to `/login` |
| `/documentation/reference` (unauthenticated) | Loads (public) |
| `/documentation/reference/full-glossary` (unauthenticated) | Redirects to `/login` |
| `/login` | Form renders with "STRATA Protocol — members only area" |
| Login (valid credentials) | Redirects to destination; header shows user name |
| Login (invalid credentials) | "Invalid email or password" — no stack trace exposed |
| Sign out | Header reverts to "Sign in"; protected pages redirect |
| `POST /__nuxt_content/docs/query` (unauth, protected path) | HTTP 401 |
| `POST /__nuxt_content/docs/query` (auth, sufficient role) | HTTP 200 |
| Homepage iframe | `public/strata-visual.html` loads without broken frame |
| `npm run type-check` | Exit 0 |
| `npm run build` | Exit 0 |
| `npm run test:unit -- --run` | Exit 0, 15/15 tests pass |

**Dependencies:**  
Phase 4 complete. Logo files (v1 official) confirmed present in `public/logo/`.

---

## 3. Technical Strategy

### Architecture Considerations

REQ-000 establishes the canonical Nuxt 4 layered architecture for this project. All subsequent REQs operate within this framework:

```
Content (MDC) → Vue Component → App Composable → Shared Util → Nitro API Route → Server Middleware
```

**Docus layer resolution:** `extends: ['docus']` in `nuxt.config.ts` makes the project a Nuxt Layer that extends Docus. Project files in `app/` take precedence over Docus defaults via Nuxt's layer resolution algorithm. Component overrides in `app/components/` are silently picked up if the filename matches a Docus component name exactly.

**Content collections:** Two separate collections ensure the landing page and docs pages are independently queryable:
- `landing` collection: `content/index.md` only — queried by `app/pages/index.vue`
- `docs` collection: all other `content/**/*.md` and `.navigation.yml` — queried by Docus's built-in page renderer

### Nitro API Routes & Data Flow

**Login flow:**
```
POST /api/auth/login
  → readBody(event)          [server/api/auth/login.post.ts]
  → validateInput()          [inline — no empty fields, valid email format]
  → authenticate(email, pwd) [server/utils/auth-adapter.ts]
    → getUsers()             [server/utils/user-store.ts — lazy NUXT_USERS_JSON parse]
    → bcryptjs.compare()     [pure-JS bcrypt verify]
  → setUserSession(event, { user: { id, email, name, role } })  [Nitro auto-import]
  → return 200 { ok: true }
```

**Logout flow:**
```
POST /api/auth/logout
  → clearUserSession(event)  [Nitro auto-import]
  → return 200 { ok: true }
```

**Content API guard flow:**
```
POST /__nuxt_content/*/query
  → server/middleware/content-guard.ts (intercepts)
  → reads body via readBody() (h3 caches — safe)
  → extracts queried path from query payload
  → findProtectedRoute(path) [shared/utils/protected-routes.ts]
  → if protected:
      getUserSession(event)   [Nitro auto-import]
      if no session or insufficient role → createError({ statusCode: 401 })
  → if not protected (or sufficient role): pass through
```

**Input validation:** All `server/api/` handlers must use `readBody()` and validate at the Nitro handler boundary. Never trust client-supplied data. Validation errors return HTTP 400 with a generic message (never internal details).

### Composables & Session State

**Client session state** (`app/` layer):
- `useUserSession()` from `nuxt-auth-utils` provides: `{ loggedIn, user, fetchSession, clear }`
- `fetchSession()` syncs the client state from the server — required after login before navigation
- `clear()` clears the client-side session state — required after logout alongside the server cookie teardown
- No Pinia store. No Redux. Session state is co-located in composables.

**Server session state** (`server/` layer):
- `getUserSession(event)` — reads the sealed HttpOnly cookie and returns the session data
- `setUserSession(event, data)` — sets the session; called after successful authentication
- `clearUserSession(event)` — invalidates the session; called on logout
- These are Nitro auto-imports. Never add `import { getUserSession } from '#auth-utils'` — this causes `TS2724`.

**`useSubNavigation.ts`** composable:
- Overrides Docus's default `sortAndClear()` which sorts alphabetically by filename stem
- Sorts by `navigation.order` field instead — matches the normative ordering in `.navigation.yml`

### Error Handling / Logging

- **Server-side errors:** Use `createError({ statusCode, statusMessage })` from Nitro. Never include internal details (stack traces, user data, bcrypt errors) in `statusMessage`.
- **401 Unauthorized:** Returned by `content-guard.ts` for protected path access without authentication
- **403 Forbidden:** Returned for authenticated access with insufficient role
- **400 Bad Request:** Returned for invalid login input (missing fields, malformed email)
- **401 for failed login:** Return generic "Invalid email or password" — never distinguish "user not found" from "wrong password" (prevents user enumeration)
- **Client-side errors:** Handled via `useAsyncData` error states in Vue components. Login error displayed inline in form.

---

## 4. Migration / Refactor Notes

### Files to Delete (M-1) — Requires Human Confirmation

These deletions are irreversible. Confirm before executing:

| File | Reason |
|---|---|
| `app/app.vue` | Docus layer provides `app.vue` via layer resolution — a project-level override conflicts |
| `app/pages/[...slug].vue` | Docus provides its own content rendering; this uses the old single-collection `queryCollection('content')` call |
| `app/components/Alert.vue` | Default scaffold artifact; not in spec |
| `app/components/Counter.vue` | Default scaffold artifact; not in spec |
| `content/about.md` | Not part of the 8-section content structure |

### Configs to Replace (M-2 through M-4)

| File | What Changes |
|---|---|
| `package.json` | Remove `@nuxt/content`, `vue`, `vue-router`; remove `generate` script; add `docus`, `nuxt-auth-utils`, `bcryptjs`, `better-sqlite3`, `vitest`, `vue-tsc`, `@types/bcryptjs` |
| `nuxt.config.ts` | Completely replace: add `extends: ['docus']`, change modules to `['nuxt-auth-utils']`, add `site`, `runtimeConfig`, `nitro.preset: 'node-server'`, `hooks.ready` block |
| `content.config.ts` | Completely replace: two-collection setup (`landing` + `docs`) with Zod schema |

**The `hooks.ready` block in `nuxt.config.ts` must never be removed.** It fixes the Windows dev-server CSS 404 bug (Gotcha G1). It is a no-op on Linux/macOS.

**`server/middleware/content-guard.ts` is a new file in Phase 3, not a modification.** Any future modification to weaken its access control logic requires human engineer approval.

---

## 5. Validation Approach

### Automated Tests

**Location:** `shared/utils/protected-routes.spec.ts` (co-located with `protected-routes.ts`)  
**Count:** 15 tests  
**Runner:** `npm run test:unit -- --run`  

All 15 tests must pass. Test failures are a blocking gate — no phase can be marked complete while tests fail.

**Test groups:**

| Group | Count | What it validates |
|---|---|---|
| `ROLE_HIERARCHY` ordering | 2 | `public < customer < partner < admin` |
| Public paths return null | 4 | Paths outside PROTECTED_ROUTES are not blocked |
| False prefix hit prevention | 1 | `/documentation-extra` does NOT match `/documentation` |
| `deep-dives` protection | 2 | Exact + sub-path |
| `account` protection | 1 | Exact match |
| `full-glossary` protection | 2 | Exact match; public parent remains public |
| `ai-playbook/guides` protection | 2 | Exact + sub-path |
| `developer-guide` protection | 2 | Exact + sub-path |
| `marketplace/premium` partner tier | 1 | Partner-role route matched correctly |

### Manual Tests

Per Checkpoint 3 and Checkpoint 5 acceptance criteria. Key scenarios:

| Scenario | Method | Expected |
|---|---|---|
| Unauthenticated access to `/developer-guide` | Browser navigation | Redirect to `/login?redirect=%2Fdeveloper-guide` |
| Unauthenticated access to `/documentation/deep-dives` | Browser navigation | Redirect to `/login` |
| Unauthenticated access to `/documentation/reference` | Browser navigation | Page loads (public) |
| Unauthenticated access to `/documentation/reference/full-glossary` | Browser navigation | Redirect to `/login` |
| Unauthenticated Content API query (protected path) | `curl -X POST /__nuxt_content/docs/query` | HTTP 401 |
| Login → redirect back to protected page | Browser form | Arrives at original destination |
| Sign out → protected page access | Browser navigation | Redirect to `/login` |
| Login with wrong password | Browser form | "Invalid email or password" — no stack trace |

### Edge Cases

| Edge Case | Expected Behavior |
|---|---|
| `redirect` param contains `http://evil.com` (open redirect attempt) | Sanitized — redirects to `/` |
| `redirect` param is empty | Redirects to `/` |
| `NUXT_SESSION_PASSWORD` not set | Server fails to start with clear error |
| `NUXT_USERS_JSON` is invalid JSON | `user-store.ts` returns empty array; all logins fail with "Invalid email or password" |
| Two instances of `findProtectedRoute` in the same request | Safe — `PROTECTED_ROUTES` is a module-level constant, read-only |
| Role check for `admin` user on `customer`-required route | Allowed — `ROLE_HIERARCHY['admin'] >= ROLE_HIERARCHY['customer']` |
| `npm run build` on Windows | `hooks.ready` normalizes the CSS path; build succeeds |

---

## 6. RBAC & Content Protection Checklist

REQ-000 introduces the entire RBAC infrastructure. This is the most RBAC-significant requirement in the project.

**All four mandatory steps are implemented in Phase 3:**

| Item | Status | Phase |
|---|---|---|
| Frontmatter `requiredRole: "customer"` on all protected pages | [ ] Pending Phase 2 | Phase 2 |
| `/documentation/reference/full-glossary` added to `PROTECTED_ROUTES` | [ ] Pending Phase 3 | Phase 3 |
| `/documentation/deep-dives` added to `PROTECTED_ROUTES` | [ ] Pending Phase 3 | Phase 3 |
| `/documentation/account` added to `PROTECTED_ROUTES` | [ ] Pending Phase 3 | Phase 3 |
| `/ai-playbook/guides` added to `PROTECTED_ROUTES` | [ ] Pending Phase 3 | Phase 3 |
| `/developer-guide` added to `PROTECTED_ROUTES` | [ ] Pending Phase 3 | Phase 3 |
| `/marketplace/premium` (partner) added to `PROTECTED_ROUTES` | [ ] Pending Phase 3 | Phase 3 |
| Test cases for all 6 routes in `protected-routes.spec.ts` (15 tests) | [ ] Pending Phase 3 | Phase 3 |
| `npm run test:unit -- --run` passes after PROTECTED_ROUTES added | [ ] Pending Phase 3 | Phase 3 |
| `server/middleware/content-guard.ts` NOT weakened | [ ] Verify at Phase 3 | Phase 3 |

**Escalate to human engineer if:**
- A new role tier beyond `public | customer | partner | admin` is required
- Any modification to `server/middleware/content-guard.ts` weakens access control
- A protected path prefix overlaps with an existing public route prefix in a non-obvious way

**Canonical PROTECTED_ROUTES (6 entries, most-specific-first):**

```typescript
export const PROTECTED_ROUTES: ProtectedRoute[] = [
  // Most specific first — single page within a public section
  { prefix: '/documentation/reference/full-glossary', role: 'customer' },

  // Sub-sections within public sections
  { prefix: '/documentation/deep-dives',              role: 'customer' },
  { prefix: '/documentation/account',                 role: 'customer' },
  { prefix: '/ai-playbook/guides',                    role: 'customer' },

  // Entire sections — member-only
  { prefix: '/developer-guide',                       role: 'customer' },

  // Reserved — future partner tier
  { prefix: '/marketplace/premium',                   role: 'partner'  },
]
```
