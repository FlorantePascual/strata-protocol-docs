# REQ-000 — Platform Bootstrap & Pre-Flight Checklist: Analysis

> **Feature:** REQ-000 — Platform Bootstrap & Pre-Flight Checklist  
> **Requirement Source:** `docs/strata/2.derivation/04-prd/REQ-000-Platform-Bootstrap-PreFlight.md`  
> **Related Features:** REQ-001 through REQ-013 (all depend on this)  
> **Date:** 2026-05-20  
> **Status:** Analysis complete — ready for implementation planning

---

## 1. Feature Summary

### Description

REQ-000 is the mandatory pre-flight that transforms the current broken repository skeleton into a fully operational Nuxt 4 / Docus documentation portal with server-side authentication and role-based content protection. It is not a "feature" in the product sense — it is the platform that all features run on.

The work spans every layer of the codebase: root configuration, `app/`, `server/`, `shared/`, `content/`, `public/`, and `scripts/`. It consists of a pre-phase migration step (removing and replacing wrong files) followed by six sequential phases building up the stack from configuration through to assets.

### Motivation

The current repository contains:

- A **prohibited `generate` script** that would bypass server-side content protection entirely if used
- **Wrong package dependencies** (`@nuxt/content` standalone, no `docus`, no auth or test tooling)
- **Wrong configuration** (`nuxt.config.ts` lacks `extends: ['docus']`, `nitro.preset`, `hooks.ready` — three critical settings)
- **Wrong `content.config.ts`** — single collection model, incompatible with the landing/docs split that RBAC requires
- **Scaffold artifacts** that must be removed before valid work can begin (`app/app.vue`, `[...slug].vue`, `Alert.vue`, `Counter.vue`, `content/about.md`)
- **Zero auth infrastructure** — no session management, no user store, no content guard, no route guard

Without REQ-000 passing all 13 acceptance criteria, no subsequent requirement has a valid execution target.

### User Impact

| User Role | Before REQ-000 | After REQ-000 |
|---|---|---|
| **Anonymous / Public** | Repository broken; site cannot start in correct mode | Site starts; 8-section navigation loads; public content accessible |
| **Customer (Member)** | No auth system; no protected content exists | Login works; member-only sections redirect correctly; session persists 7 days |
| **Partner** | No auth system | Auth system ready; partner tier enforced at `/marketplace/premium` |
| **Admin** | No auth system | Auth system operational; admin role hierarchy in place |
| **Developer / Operator** | No type-checking, no test runner, no build validation | `type-check`, `build`, `test:unit` all exit 0; gotchas documented and handled |

### Mission Alignment

**Product Value engaged: *Governed AI-Assisted Development*** — REQ-000 itself was governed by the STRATA Protocol: classified, derived from the BRD, planned with a phase map, and constrained by the operative constitution. The platform that teaches STRATA governance was built by following it.

**Product Value engaged: *Artifact Trail* [G:Strata]** — The security findings (§3.3), canonical decisions table, and known-gotchas summary in REQ-000 constitute the authoritative artifact trail for all architectural decisions made at bootstrap time.

**BRD sections engaged:** BR-001 (platform), BR-004 (auth), BR-010 (RBAC), BR-011 (quality gates) — REQ-000 is a prerequisite for all 13 BRD requirements.

---

## 2. Repo-Based Structural Analysis

### 2.1 Existing Codebase Context

The following table documents the **actual current state** of the repository as verified against the live file system:

| Path | Current State | Required State | Action |
|---|---|---|---|
| `package.json` | `@nuxt/content` dep; `generate` script; no docus/auth/test | Per REQ-000 §M-2 | **REPLACE** |
| `nuxt.config.ts` | `modules: ['@nuxt/content']`; no `extends`; no `hooks.ready`; no `nitro.preset` | Per REQ-000 §M-3 | **REPLACE** |
| `content.config.ts` | Single `content` collection, `source: '**'` | Landing + docs split per REQ-000 §M-4 | **REPLACE** |
| `tsconfig.json` | Four-reference Nuxt 4 structure | Confirmed correct per REQ-000 §M-5 | **NO CHANGE** |
| `app/app.vue` | Bare-bones pass-through | DELETE — Docus provides via layer resolution | **DELETE** |
| `app/pages/[...slug].vue` | `queryCollection('content')` (old single-collection) | DELETE — Docus layer provides its own | **DELETE** |
| `app/components/Alert.vue` | Default scaffold artifact | Not in spec | **DELETE** |
| `app/components/Counter.vue` | Default scaffold artifact | Not in spec | **DELETE** |
| `content/about.md` | Not in 8-section content structure | Not in spec | **DELETE** |
| `content/index.md` | Bare stub | STRATA homepage with hero + feature grid | **REPLACE (Phase 2)** |

### 2.2 Feature Directory (new files introduced)

REQ-000 introduces the largest single batch of files in this project's lifecycle. They are organized by layer:

```
# Root configuration
tsconfig.typecheck.json          ← NEW (Phase 0)
vitest.config.ts                 ← NEW (Phase 0)
.env.example                     ← NEW (Phase 0)

# Scripts
scripts/
  type-check.mjs                 ← NEW (Phase 4)

# App layer
app/
  app.config.ts                  ← NEW (Phase 1) — midnight theme + STRATA branding
  app.css                        ← NEW (Phase 1)
  types/
    auth.d.ts                    ← NEW (Phase 1) — augments #auth-utils for IDE tooling
  composables/
    useSubNavigation.ts          ← NEW (Phase 1) — REQUIRED navigation order fix
  middleware/
    auth-guard.global.ts         ← NEW (Phase 3) — two-phase client route guard
  pages/
    index.vue                    ← NEW (Phase 1) — landing page with function-based key fix
    login.vue                    ← NEW (Phase 3) — STRATA-branded login form
  components/
    AppFooterLeft.vue            ← NEW (Phase 1) — STRATA copyright notice
    AppHeaderBottom.vue          ← NEW (Phase 1) — REQUIRED for auth controls to appear
    AppHeaderBottomRight.vue     ← NEW (Phase 1) — sign-in / sign-out controls

# Shared layer
shared/
  types/
    auth.d.ts                    ← NEW (Phase 3) — server-context type checking
  utils/
    protected-routes.ts          ← NEW (Phase 3) — 6 PROTECTED_ROUTES entries
    protected-routes.spec.ts     ← NEW (Phase 3) — 15 tests (2 hierarchy + 13 route)

# Server layer
server/
  tsconfig.json                  ← NEW (Phase 3) — extends .nuxt/tsconfig.server.json
  utils/
    user-store.ts                ← NEW (Phase 3) — MVP JSON user store with bcryptjs
    auth-adapter.ts              ← NEW (Phase 3) — local/remote strategy dispatch
  api/
    auth/
      login.post.ts              ← NEW (Phase 3) — server-side login handler
      logout.post.ts             ← NEW (Phase 3) — session teardown
  middleware/
    content-guard.ts             ← NEW (Phase 3) — guards /__nuxt_content/ queries

# Content layer (48 files)
content/
  index.md                       ← REPLACE (Phase 2) — STRATA homepage
  documentation/
    .navigation.yml              ← NEW (Phase 2)
    index.md                     ← NEW (Phase 2)
    introduction/index.md        ← NEW (Phase 2) — Finding 3 correction
    getting-started/index.md     ← NEW (Phase 2)
    the-strata/
      index.md                   ← NEW (Phase 2)
      stratum-{1-5}-*/index.md   ← NEW ×5 (Phase 2)
    concepts/index.md            ← NEW (Phase 2)
    reference/
      index.md                   ← NEW (Phase 2) — public
      full-glossary.md           ← NEW (Phase 2) — customer [Finding 1]
    deep-dives/
      index.md                   ← NEW (Phase 2) — customer [Finding 2]
      stratum-{1-5}-*/index.md   ← NEW ×5 (Phase 2) — customer
    account/index.md             ← NEW (Phase 2) — customer
  developer-guide/
    .navigation.yml              ← NEW (Phase 2) — title: "Templates" [Finding 4]
    index.md + 7 sub-stubs       ← NEW (Phase 2) — all customer
  case-studies/
    .navigation.yml + 5 stubs    ← NEW (Phase 2) — all public
  ai-playbook/
    .navigation.yml + 4 stubs    ← NEW (Phase 2) — mixed (guides: customer)
  video-courses/
    .navigation.yml + index.md   ← NEW (Phase 2) — public
  marketplace/
    .navigation.yml + index.md   ← NEW (Phase 2) — public
  blog/
    .navigation.yml + index.md   ← NEW (Phase 2) — public
  changelog/
    .navigation.yml + index.md   ← NEW (Phase 2) — public, actual content

# Public assets
public/
  logo/
    favicon.png                  ← NEW (Phase 5) — 32×32 or 64×64 PNG
    logo-dark.png                ← NEW (Phase 5)
    logo-light.png               ← NEW (Phase 5)
  strata-visual.html             ← NEW (Phase 5) — cleaned from docs/about/strata.html
```

**Total new files:** ~75 (48 content + 17 app/server/shared + 7 public/config/scripts)  
**Files replaced:** 4 core config files + `content/index.md`  
**Files deleted:** 5

### 2.3 Architecture & Patterns

REQ-000 establishes every layer in the mandatory stack architecture:

```
Content (MDC) → Vue Component → App Composable → Shared Util → Nitro API Route → Server Middleware
```

**Layer by layer:**

| Layer | Established by REQ-000 | Pattern |
|---|---|---|
| Content | 48 stub Markdown pages with RBAC frontmatter | MDC syntax, `requiredRole` frontmatter, `.navigation.yml` ordering |
| App Composable | `useSubNavigation.ts` | Overrides Docus default navigation sort to use `navigation.order` |
| App Pages | `index.vue`, `login.vue` | Function-based query key (G3 fix); `fetchSession()` after login (G6 fix) |
| App Middleware | `auth-guard.global.ts` | Two-phase: Phase A fast-path via `PROTECTED_ROUTES`; Phase B precise role via `queryCollection` |
| Shared Utils | `protected-routes.ts` | `findProtectedRoute()` — consumed by both `auth-guard` (app) and `content-guard` (server) |
| Server API | `login.post.ts`, `logout.post.ts` | `readBody()` validation → `bcryptjs.compare()` → `setUserSession()` auto-import |
| Server Middleware | `content-guard.ts` | Intercepts `POST /__nuxt_content/*/query`; calls `findProtectedRoute()` |

**Session state** is managed exclusively via `useUserSession()` (client) and `getUserSession()` / `setUserSession()` / `clearUserSession()` (server — Nitro auto-imports, never explicit imports).

**No Pinia store.** Auth state flows from `nuxt-auth-utils` composables. No Redux, no Vuex.

### 2.4 Integration Points

| Symbol / Contract | Consumer | Provider | Notes |
|---|---|---|---|
| `findProtectedRoute(path)` | `server/middleware/content-guard.ts` | `shared/utils/protected-routes.ts` | Critical security boundary — used for server-side content API guard |
| `findProtectedRoute(path)` | `app/middleware/auth-guard.global.ts` | `shared/utils/protected-routes.ts` | Client route guard Phase A fast-path |
| `getUserSession(event)` | `server/middleware/content-guard.ts`, `server/api/auth/login.post.ts` | Nitro auto-import (nuxt-auth-utils) | Never import explicitly — see copilot-instructions.md §7 |
| `setUserSession(event, data)` | `server/api/auth/login.post.ts` | Nitro auto-import | Sets sealed HttpOnly session cookie |
| `clearUserSession(event)` | `server/api/auth/logout.post.ts` | Nitro auto-import | Tears down server-side session |
| `useUserSession()` | `app/middleware/auth-guard.global.ts`, `AppHeaderBottomRight.vue` | nuxt-auth-utils composable | Client-side session state; `loggedIn`, `user`, `fetchSession()`, `clear()` |
| `authenticate(email, password)` | `server/api/auth/login.post.ts` | `server/utils/auth-adapter.ts` | Dispatches to local (NUXT_USERS_JSON) or remote (NUXT_AUTH_API_URL) strategy |
| `getUsers()` | `server/utils/auth-adapter.ts` | `server/utils/user-store.ts` | Parses NUXT_USERS_JSON; never called on server startup (lazy) |
| `PROTECTED_ROUTES` (array) | `findProtectedRoute` | `shared/utils/protected-routes.ts` | Consumed by both app and server layers — must stay in `shared/` |
| `queryCollection('landing')` | `app/pages/index.vue` | @nuxt/content (via Docus) | Function-based key pattern required (G3 fix) |
| `NUXT_SESSION_PASSWORD` | nuxt-auth-utils session sealing | Environment variable | Never committed; minimum 32 chars; required for any deployment |
| `NUXT_USERS_JSON` | `server/utils/user-store.ts` | Environment variable | JSON array of `StoredUser` records; bcrypt-hashed passwords |

### 2.5 Scope Boundaries

REQ-000 creates **stubs and scaffolding** only. Content authoring is owned by downstream REQs:

| Responsibility | Owner |
|---|---|
| Public framework documentation prose | **REQ-001** (`content/documentation/the-strata/`) |
| Architecture visualization (`strata-visual.html` content) | **REQ-002** — REQ-000 only cleans and places the file |
| Getting started content | **REQ-003** (`content/documentation/getting-started/`) |
| Glossary content | **REQ-004** (`content/documentation/reference/`) |
| Deep-dive member documentation | **REQ-005** (`content/documentation/deep-dives/`) |
| Implementation templates | **REQ-006** (`content/developer-guide/`) |
| AI Playbook guides | **REQ-007** (`content/ai-playbook/guides/`) |
| Auth invitation flows | **REQ-009** (extends `server/utils/user-store.ts`, `server/utils/auth-adapter.ts`) |
| Case studies content | **REQ-013** (`content/case-studies/`) |
| PROTECTED_ROUTES expansion | **Any REQ that adds a new protected path prefix** (per RBAC Integrity Rule) |

---

## 3. Functional & Non-Functional Considerations

### 3.1 Functional Logic

REQ-000 establishes six distinct functional capabilities:

**1. Dependency and build layer** (Migration + Phase 0):  
Replace the broken package manifest and configuration to enable `docus`-based layered architecture. Without this, `npm run dev` loads `@nuxt/content` in standalone mode — Docus components, theming, and layout are unavailable.

**2. STRATA branding and navigation** (Phase 1):  
The `midnight` theme (violet/zinc), STRATA header title, copyright footer, and correct navigation ordering. The `useSubNavigation.ts` override is functionally required — without it, Nuxt Content's default `sortAndClear()` orders navigation alphabetically by filename, not by `navigation.order`. All 8 section tabs would appear in the wrong order.

**3. Content scaffold** (Phase 2):  
48 files establishing the complete 8-section content tree. All protected pages have `requiredRole` frontmatter from day one — even though auth enforcement only activates in Phase 3. This prevents a situation where Phase 3 is deployed but stub pages have no role declarations.

**4. Auth system** (Phase 3):  
The complete session-based authentication stack:
- Login: `POST /api/auth/login` → validate input → `authenticate()` → `setUserSession()` → 200
- Logout: `POST /api/auth/logout` → `clearUserSession()` → 200  
- Client guard: `auth-guard.global.ts` — blocks navigation to protected routes for unauthenticated users
- Server guard: `content-guard.ts` — blocks direct Content API queries for protected paths (prevents bypass of client guard via direct API call)

The two-step sign-out sequence in `AppHeaderBottomRight.vue` is a functional requirement: `$fetch('/api/auth/logout')` (server cookie teardown) → `clear()` (client state sync). Missing either step leaves the session partially alive.

**5. Type-checking and testing infrastructure** (Phase 4):  
The dual-pass `scripts/type-check.mjs` runs `vue-tsc` in app context and `tsc` in server context separately, matching the split tsconfig references in `tsconfig.json`. A single-pass type-check would fail because Docus has pre-existing strict-mode violations in `node_modules/` that must be suppressed.

**6. Assets** (Phase 5):  
`public/strata-visual.html` — cleaned from `docs/about/strata.html` by removing external font URLs (Anthropic CDN) and replacing with `system-ui`. Without this file, the homepage iframe renders broken.

### 3.2 UX and Interaction Impacts

| Element | Behavior | Notes |
|---|---|---|
| Header tabs | 8 sections in defined order: Documentation → Changelog | Requires `useSubNavigation.ts` override |
| Header auth control | "Sign in" (unauthenticated); username + "Sign out" (authenticated) | Requires `AppHeaderBottom.vue` + `AppHeaderBottomRight.vue` overrides |
| Login redirect | `/login?redirect=%2Fdeveloper-guide` — round-trips to destination | `redirect` param sanitized against allow-list; SSRF not possible |
| Protected page access | Redirect to `/login` — no content flash | Client guard fires before page mount |
| Content API protection | `POST /__nuxt_content/docs/query` with protected path → 401 | Server middleware intercepts before any data is returned |
| Navigation ordering | Documentation (1) → Templates (2) → ... → Changelog (8) | `navigation.order` in `.navigation.yml` + `useSubNavigation.ts` |
| Dark/light mode | System preference; toggle in header | Inherited from Docus layer — no customization needed |
| Sidebar | Shows section structure from `.navigation.yml` | Icons and titles per Phase 2 spec |
| Breadcrumbs | Auto-generated from path hierarchy | Docus-provided |

### 3.3 Security Considerations

**Threat: SSG bypass of content guard**  
*Severity: Critical.* If `nuxt generate` were run, all content (including member-only deep-dives) would be rendered to static HTML files accessible without authentication. The `node-server` preset in `nuxt.config.ts` prevents this — the `generate` script removal is a security control, not just a housekeeping item.

**Threat: Direct Content API bypass of client route guard**  
*Severity: High.* An unauthenticated user can call `POST /__nuxt_content/docs/query` directly, bypassing the browser-side `auth-guard.global.ts`. `server/middleware/content-guard.ts` is the server-side defense. It uses `findProtectedRoute()` to check the queried path before Nitro serves any content.

**Threat: Missing server-side protection for `full-glossary.md`**  
*Severity: Medium.* REQ-004 originally specified frontmatter-only protection for `/documentation/reference/full-glossary`. REQ-000 Finding 1 corrects this as a security violation: the entry is added to `PROTECTED_ROUTES` so the Content API guard applies. This is a canonical decision that overrides REQ-004.

**Threat: Session credential exposure**  
`NUXT_SESSION_PASSWORD` seals the session cookie. If committed, anyone with the secret can forge session cookies. Must be in `.gitignore`. Minimum 32 characters. The `.env.example` file documents this requirement without including any real value.

**Threat: bcrypt timing bypass**  
`bcryptjs.compare()` is used for password verification — not string equality. `server/utils/auth-adapter.ts` must never use a plain `===` comparison for passwords.

**Threat: Open redirect on login**  
The `redirect` parameter in `/login?redirect=...` must be sanitized. Only relative paths starting with `/` (not `//`) and not containing `://` are allowed. Per spec §8.10 and copilot-instructions.md §7.

**Auth session pattern:**  
- Shape: `{ user: { id, email, name?, role } }`  
- Roles: `'public' | 'customer' | 'partner' | 'admin'`  
- TTL: 7 days (`session.maxAge: 60 * 60 * 24 * 7`)  
- Cookie: sealed HttpOnly — never exposed client-side  

### 3.4 Performance Considerations

- **`bcryptjs` (pure-JS):** ~100ms per verify at cost factor 10. Acceptable for login; unacceptable if called on every request. `server/middleware/content-guard.ts` must NOT call `bcryptjs` — it only checks the existing session cookie.
- **`better-sqlite3`:** Synchronous I/O, very fast for the Nuxt Content search index (its sole use here). Not used for auth — the MVP user store is environment-variable JSON.
- **SSR on every request:** No static caching. Nuxt Content serves from SQLite. For the scale of a documentation site this is fine; horizontal scaling would require sticky sessions or shared session storage (deferred to future REQs).
- **`useSubNavigation.ts`:** Sorts navigation on each page render. With 8 top-level items it is negligible.
- **`content-guard.ts`:** Intercepts every `POST /__nuxt_content/*/query`. The `findProtectedRoute()` lookup is O(n) over 6 entries — negligible overhead.

### 3.5 Accessibility

- **Login form:** Must have proper `<label>` elements for all inputs, not just `placeholder` attributes. `aria-invalid` on validation error states.
- **Theme colors:** The `midnight` theme (violet/zinc) uses Nuxt UI / Docus tokens. Color contrast is managed by the framework; custom overrides in `app.config.ts` should be verified with WCAG 2.1 AA at minimum.
- **Interactive header controls:** Sign in / Sign out button must be keyboard accessible (inherited from Docus component patterns).
- **Reduced motion:** No custom animations in REQ-000 scope. Framework defaults apply.

### 3.6 Content & RBAC Considerations

REQ-000 introduces the **complete RBAC infrastructure** and **6 protected path prefixes**. This is the most RBAC-significant requirement in the project.

**Protected pages introduced:**

| Path Prefix | Role Required | Finding / Note |
|---|---|---|
| `/documentation/reference/full-glossary` | `customer` | Finding 1 — security correction overriding REQ-004 |
| `/documentation/deep-dives` | `customer` | Finding 2 — STRATA-specific, not in DOCUS generic spec |
| `/documentation/account` | `customer` | Per BRD |
| `/ai-playbook/guides` | `customer` | Per BRD |
| `/developer-guide` | `customer` | Per BRD |
| `/marketplace/premium` | `partner` | Reserved for future partner tier |

**RBAC four-step checklist:**

- [x] Protected pages have `requiredRole` frontmatter *(implemented in Phase 2)*
- [x] `PROTECTED_ROUTES` entries added in `shared/utils/protected-routes.ts` *(implemented in Phase 3)*
- [x] Test cases added to `shared/utils/protected-routes.spec.ts` (15 tests) *(implemented in Phase 3)*
- [x] `npm run test:unit -- --run` required to pass after Phase 3

**Critical ordering note:** `PROTECTED_ROUTES` must be sorted **most-specific-first** (longer prefixes before shorter). `/documentation/reference/full-glossary` must appear before `/documentation/account` to ensure `findProtectedRoute('/documentation/reference/full-glossary')` matches the specific entry, not a shorter public prefix.

`server/middleware/content-guard.ts` must NOT be modified without explicit human engineer approval per copilot-instructions.md §12.

---

## 4. Risk Analysis

### 4.1 Technical Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Windows dev-server CSS 404 (Docus registers `docus.css` with absolute Windows path) | High (Windows env) | High (site fails to load in dev) | `hooks.ready` block in `nuxt.config.ts` — **never remove**; see Gotcha G1 |
| Navigation order ignored (Nuxt Content sorts alphabetically by default) | High | Medium (wrong tab order; UX broken) | `useSubNavigation.ts` override — **required**; see Gotcha G2 |
| Landing page stale null on client navigation | High (without fix) | Medium (blank homepage on back-nav) | Function-based key in `app/pages/index.vue`; see Gotcha G3 |
| `getUserSession` imported explicitly causing TS2724 | Medium | High (type error; build failure) | Never import — use as Nitro auto-import; see Gotcha G5 |
| `fetchSession()` missing after login causing infinite redirect loop | Medium | High (login succeeds but returns to login) | Always call `fetchSession()` before `navigateTo()` in login.vue; see Gotcha G6 |
| SSG deployment bypassing content guard | Low (config-enforced) | Critical (security breach) | `nitro.preset: 'node-server'` + no `generate` script; see Gotcha G11 |
| `.nuxt/` directory absent when type-check runs | High (first run) | High (type-check fails; misleading errors) | Must run `nuxt dev` (Step M-7) before Phase 0 begins |

### 4.2 Dependency Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| `@nuxt/content` or `@nuxt/ui` added as explicit deps (conflicts with Docus) | Low (if following spec) | High (package resolution conflict; build failure) | `package.json` review gate; prohibited in copilot-instructions.md §13 |
| `docus: latest` includes a breaking change | Low | Medium (build fails after npm install) | Pin `docus` version after first successful build; create lockfile |
| `nuxt-auth-utils` `^0.5` — minor version breaking change | Low | Medium (session methods behave differently) | Lock to exact version after first successful build |
| `bcryptjs ^3.0.3` — pure JS slower than native bindings | Low | Low (acceptable for MVP scale) | Deferred; switch to `argon2` for production hardening if needed |
| `better-sqlite3 ^12.2.0` — native bindings require matching Node.js ABI in Docker | Medium | Medium (deployment failure) | Ensure Docker base image Node.js version matches dev environment |

### 4.3 Architectural Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Two-step sign-out partially implemented (cookie cleared but not `clear()`, or vice versa) | Medium | Medium (session ghost / UX confusion) | Enforce both steps in `AppHeaderBottomRight.vue`; add to manual test checklist |
| `content-guard.ts` modified to weaken protection | Low | Critical | Human-approval gate per copilot-instructions.md §12 |
| Horizontal scaling with sticky sessions | Low (single instance MVP) | Medium (session cookie not shared) | Out of scope for REQ-000; document as known constraint |
| NUXT_SESSION_PASSWORD committed in `.env` | Low | Critical | `.gitignore` enforced; `.env.example` documents requirement |
| `content/index.md` moved to `docs` collection | Low | Medium (RBAC anomaly; content routing error) | `content.config.ts` excludes `index.md` from docs; landing collection is separate |

---

## 5. Dependencies

### 5.1 Mandatory Predecessors

REQ-000 has **no mandatory predecessor REQs** — it is the foundation. However, it has environmental prerequisites:

| Prerequisite | Reason |
|---|---|
| Node.js ≥18 (LTS) | Required by `better-sqlite3` native bindings and Nuxt 4 |
| `npm` available | Package manager for `npm install` |
| `docs/about/strata.html` present | Source for `public/strata-visual.html` cleanup (Phase 5) |
| `NUXT_SESSION_PASSWORD` (≥32 chars) | Required for any auth session to seal; must be operator-generated, never committed |

### 5.2 Soft Prerequisites

| Prerequisite | Degradation if Absent |
|---|---|
| Production logo files (`logo-dark.png`, `logo-light.png`, `favicon.png`) | Header and browser tab show broken images; site functional otherwise |
| `public/strata-visual.html` content quality | Homepage iframe may show a visually degraded visualization; site functional otherwise |

### 5.3 Internal Module Dependencies

| Module | Consumed By | Purpose |
|---|---|---|
| `shared/utils/protected-routes.ts` | `app/middleware/auth-guard.global.ts` | Client route guard Phase A fast-path lookup |
| `shared/utils/protected-routes.ts` | `server/middleware/content-guard.ts` | Server-side Content API guard — security boundary |
| `server/utils/user-store.ts` | `server/utils/auth-adapter.ts` | User lookup from `NUXT_USERS_JSON` environment variable |
| `server/utils/auth-adapter.ts` | `server/api/auth/login.post.ts` | Entry point for authentication strategy dispatch |
| `app/composables/useSubNavigation.ts` | Docus navigation system | Injects `navigation.order` sorting into sidebar/tab rendering |
| `app/components/AppHeaderBottom.vue` | Docus header rendering | Mount point for `AppHeaderBottomRight` — without this, auth controls never appear |

### 5.4 External Dependencies

| Library | Version Constraint | Purpose | Notes |
|---|---|---|---|
| `docus` | `latest` | Nuxt 4 documentation framework; bundles `@nuxt/content` v3 and `@nuxt/ui` | Never add `@nuxt/content` or `@nuxt/ui` separately |
| `nuxt` | `^4.3.1` | Core framework | Bundled by Docus; explicit dep ensures version control |
| `nuxt-auth-utils` | `^0.5.29` | Session management (`useUserSession`, `getUserSession`, etc.) | NOT bundled by Docus — added explicitly |
| `bcryptjs` | `^3.0.3` | Password hashing — pure JS, no native bindings | Runtime dep (not dev) |
| `better-sqlite3` | `^12.2.0` | SQLite for Nuxt Content search index | Runtime dep (not dev); requires matching Node.js ABI |
| `vitest` | `^4.1.6` | Test runner | devDependency |
| `vue-tsc` | `^3.2.9` | Vue-aware TypeScript type checker | devDependency |
| `@types/bcryptjs` | `^2.4.6` | Type definitions for bcryptjs | devDependency |

### 5.5 Feature Flags (RBAC Roles)

Access control is enforced via RBAC roles rather than feature flags:

| Route Prefix | Required Role | Enforcement Layer |
|---|---|---|
| `/documentation/reference/full-glossary` | `customer` | Frontmatter + PROTECTED_ROUTES + content-guard |
| `/documentation/deep-dives` | `customer` | Frontmatter + PROTECTED_ROUTES + content-guard |
| `/documentation/account` | `customer` | Frontmatter + PROTECTED_ROUTES + content-guard |
| `/ai-playbook/guides` | `customer` | Frontmatter + PROTECTED_ROUTES + content-guard |
| `/developer-guide` | `customer` | Frontmatter + PROTECTED_ROUTES + content-guard |
| `/marketplace/premium` | `partner` | Frontmatter + PROTECTED_ROUTES + content-guard |

Role hierarchy (ascending): `public` (0) < `customer` (1) < `partner` (2) < `admin` (3)  
A user with `admin` role satisfies a `customer` requirement. `ROLE_HIERARCHY` in `protected-routes.ts` implements this.

---

## 6. Open Questions and Decisions

### Q1 — Placeholder logos for Phase 5

**Context:** Production logo files are not in the repository. Phase 5 requires `public/logo/favicon.png`, `logo-dark.png`, and `logo-light.png`.  
**Options:**  
  A. Create minimal placeholder PNGs (solid rectangle with "STRATA" text) for immediate Phase 5 completion  
  B. Defer Phase 5 until production logo assets are provided by a designer  
**Impact:** The site renders without error in both cases (broken image tags do not crash the app). Deferred Phase 5 only affects visual polish, not functionality or security.  
**Recommendation:** Option A for development; replace with production assets before public launch. This is an operator decision.

### Q2 — `NUXT_USERS_JSON` provisioning for development

**Context:** The user store reads from `NUXT_USERS_JSON` — a JSON array of users with bcrypt-hashed passwords. No user record exists in the repository.  
**Impact:** Auth system will be fully functional but login cannot be tested until at least one user record is provisioned.  
**Resolution:** Add at least one dev test user with a known bcrypt hash to `.env` (local only, not committed). Document format in `.env.example`. Production users are provisioned per REQ-009.

### Q3 — `docs/about/strata.html` cleanup scope for Phase 5

**Context:** `docs/about/strata.html` is the source for `public/strata-visual.html`. It was originally created with Anthropic Sans font-face declarations referencing `https://assets.claude.ai/` CDN URLs. These are external assets that may not load in the STRATA domain context.  
**Options:**  
  A. Remove only the `@font-face` declarations and replace font stacks with `system-ui`  
  B. Full audit removing all external resource references  
**Recommendation:** Option B — a full audit for any `http://` or `https://` references is a security-hygiene step that prevents unintended external network requests from the user's browser when viewing the homepage.

### Q4 — `nuxt.config.ts` `compatibilityDate` field

**Context:** The current `nuxt.config.ts` has `compatibilityDate: '2024-04-03'` and `devtools: { enabled: true }`. These are not in the spec.  
**Decision:** The spec's replacement `nuxt.config.ts` is treated as authoritative. `devtools: { enabled: true }` should be retained for development convenience (not a spec violation); `compatibilityDate` should remain if present in the spec or be removed. **Escalate to human engineer if the spec §7.3 version omits both fields.**

---

## 7. References

| Document | Path |
|---|---|
| Requirement Document | `docs/strata/2.derivation/04-prd/REQ-000-Platform-Bootstrap-PreFlight.md` |
| Canonical Spec (verbatim file content) | `DOCUS-PORTAL-STARTER-SPEC.md` |
| BRD | `docs/strata/2.derivation/03-brd/business-requirements.md` |
| Vision & Business Case | `docs/strata/2.derivation/01-vision/vision-business-case.md` |
| Operative Constitution | `.github/copilot-instructions.md` |
| UI Standards | `docs/strata/2.derivation/10-engineering-standards/ui-standards.md` |
| Testing Standards | `docs/strata/2.derivation/10-engineering-standards/testing-standards.md` |
| Global Glossary | `docs/strata/glossary.md` |
| Initial Commit Analysis | `docs/strata/0.initial-commit/analysis.md` |

**Glossary Terms Used:**  
[G:RBAC] — Role-based access control  
[G:Session] — Session management via nuxt-auth-utils  
[G:ProtectedRoute] — Server-enforced protected route entry in `PROTECTED_ROUTES`  
[G:Strata] — The five-layer STRATA protocol framework  
[G:MDC] — Markdown Components (Docus/Nuxt Content syntax)
