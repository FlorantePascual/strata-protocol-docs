# REQ-000 — Platform Bootstrap & Pre-Flight Checklist

**Stratum:** 2 — Derivation, Document 04  
**Layer:** 3 — How (Product Specification)  
**Status:** Normative  
**Traces to:** All BRD requirements (BR-001 through BR-013) — pre-requisite for all  
**Derived from:** `DOCUS-PORTAL-STARTER-SPEC.md`, `docs/strata/0.initial-commit/analysis.md`, `docs/strata/0.initial-commit/implementation-plan.md`  
**Signed:** 2026-05-19  

---

## Purpose

This document is the **pre-flight checklist** for StrataProtocol.org. It must be completed — all phases passing their validation checkpoints — before any work governed by REQ-001 through REQ-013 begins.

REQ-001 through REQ-013 specify *content* and *feature* requirements against an assumed working platform. REQ-000 specifies the platform itself: the project scaffold, configuration, app layer, content structure stubs, auth system, CI toolchain, and branding. Without REQ-000 passing its acceptance criteria, none of the subsequent REQs have a valid execution target.

**Canonical source for verbatim file content:** `DOCUS-PORTAL-STARTER-SPEC.md` (root of workspace). Where this document specifies a file as "verbatim from spec §{N}", the full file content is in that section of the spec. This document specifies the STRATA-specific adaptations, the sequencing, the migration steps, and any deviations.

---

## 1. Upstream Review: Findings and Resolutions

This section documents findings from a comprehensive review of all 13 REQ documents against the source documents (`DOCUS-PORTAL-STARTER-SPEC.md`, `analysis.md`, `implementation-plan.md`, `copilot-instructions.md`). These findings informed the design of REQ-000.

### Finding 1 — Security: `/documentation/reference/full-glossary` Not Covered by Server-Side Guard

**Severity:** Medium — BR-RULE-01 compliance gap  
**Location:** REQ-004 §PROTECTED_ROUTES Decision

**Issue:** REQ-004 explicitly chose frontmatter-only gating for `/documentation/reference/full-glossary.md` (the member full glossary). The reasoning was "sub-page within a public section, not a subtree." However, `server/middleware/content-guard.ts` uses `findProtectedRoute()` which matches PROTECTED_ROUTES prefixes. Since `/documentation/reference/full-glossary` is not in PROTECTED_ROUTES, an unauthenticated user can call:

```
POST /__nuxt_content/docs/query
{"sql": "SELECT ... WHERE \"path\" = '/documentation/reference/full-glossary'"}
```

...and receive the full glossary content without authentication. This violates BR-RULE-01: "Content access must be enforced at both the browser level (route guard) and the server level (Content API guard)."

**Resolution:** Add the following entry to PROTECTED_ROUTES in `shared/utils/protected-routes.ts`:

```typescript
{ prefix: '/documentation/reference/full-glossary', role: 'customer' },
```

This entry must be placed **before** `{ prefix: '/documentation/account', role: 'customer' }` (most-specific-first ordering per G10 in the spec). The corresponding test case is specified in Phase 3 below.

**REQ-004 status:** The PRD text does not need to be amended — REQ-000 supersedes it on this specific point as a security correction. This is documented in the Canonical Decisions table at the end of this document.

---

### Finding 2 — Content Scaffold: `documentation/deep-dives/` Missing from Phase 2

**Severity:** Implementation blocker — ordering gap  
**Location:** REQ-005 vs. DOCUS spec Phase 2 sequence

**Issue:** The DOCUS spec Phase 2 creates all content scaffold files. The `documentation/deep-dives/` section (defined by REQ-005) is STRATA-specific and was not in the DOCUS generic spec's content tree. An implementor following Phase 2 literally from the DOCUS spec would omit it.

**Resolution:** Phase 2 in this document includes stub pages for `documentation/deep-dives/` with `requiredRole: "customer"` from day one. The actual content is authored per REQ-005, but the structure and RBAC stubs must exist at Phase 2 completion.

---

### Finding 3 — REQ-001 Content Tree Omits `introduction/` Sub-Section

**Severity:** Minor — navigational gap  
**Location:** REQ-001 §Content Structure

**Issue:** REQ-001 defines the Documentation section structure but does not explicitly list `documentation/introduction/` — even though both the DOCUS spec (§12.4) and the `analysis.md` (§5.2) include it. The implementation plan's content tree in §6.1 explicitly shows `introduction/`.

**Resolution:** REQ-000 Phase 2 includes `content/documentation/introduction/index.md` as a required stub. REQ-001 content requirements apply to this page (it serves as "What STRATA is, why it exists").

---

### Finding 4 — Developer Guide Section Has Display Title "Templates" in Nav

**Severity:** Minor — display / navigation consistency  
**Location:** analysis.md §5.1, implementation-plan.md §6.2

**Issue:** The DOCUS spec generic name for this section is "Developer Guide" but the STRATA-specific adaptation renames it to "Templates" in the navigation display (the URL path `/developer-guide/` remains unchanged). This affects the `.navigation.yml` for this section.

**Resolution:** Phase 2 uses `title: Templates` in `content/developer-guide/.navigation.yml`, not `title: Developer Guide`.

---

### Finding 5 — `protected-routes.spec.ts` Must Test All 5 STRATA Routes

**Severity:** Test coverage gap  
**Location:** REQ-010 §Test Cases, DOCUS spec §9.2

**Issue:** The DOCUS spec §9.2 provides 7 test cases for the baseline 3-route configuration. The STRATA configuration has 5 routes plus the newly-added full-glossary entry (6 routes total). REQ-010 specifies 13 required test cases. These 13 cases are defined verbatim in Phase 3 of this document.

**Resolution:** Phase 3 specifies the full STRATA `protected-routes.spec.ts` with all 13 test cases.

---

### Finding 6 — Existing Repository Has Multiple Invalid Files

**Severity:** Implementation blocker — must be corrected before Phase 0  
**See:** §2 Current Repository Audit and §3 Migration Action Plan

---

## 2. Current Repository Audit

The following is an exact audit of the current state of the repository against the DOCUS spec requirements:

### 2.1 Files That Are WRONG (exist but have incorrect content)

| File | Current State | Required State |
|---|---|---|
| `package.json` | Uses `@nuxt/content` directly; has `generate` script (PROHIBITED); missing `docus`, `nuxt-auth-utils`, `bcryptjs`, `better-sqlite3`, `vitest`, `vue-tsc`, `@types/bcryptjs` | Per spec §7.1 with STRATA name |
| `nuxt.config.ts` | Has `modules: ['@nuxt/content']`; missing `extends: ['docus']`; missing `runtimeConfig`, `nitro.preset`, `hooks.ready`, `site` | Per spec §7.3 with STRATA name |
| `content.config.ts` | Single `content` collection with `source: '**'`; missing `landing` / `docs` split; missing schema | Per spec §7.4 |
| `app/pages/[...slug].vue` | Uses `queryCollection('content')` (old single-collection) | DELETE — Docus layer provides its own; spec does not include a custom catch-all |
| `app/app.vue` | Bare-bones pass-through — not in spec | DELETE — Docus layer provides app.vue through layer resolution |

### 2.2 Files That Should NOT Exist

| File | Reason |
|---|---|
| `app/components/Alert.vue` | Not in spec; default scaffold artifact |
| `app/components/Counter.vue` | Not in spec; default scaffold artifact |
| `content/about.md` | Not in the 8-section content structure |

### 2.3 Missing Files (in spec, not in repo)

**Configuration layer:**
- `tsconfig.typecheck.json`
- `vitest.config.ts`
- `.env.example`

**Scripts:**
- `scripts/type-check.mjs`

**App layer:**
- `app/app.config.ts`
- `app/app.css`
- `app/types/auth.d.ts`
- `app/composables/useSubNavigation.ts`
- `app/middleware/auth-guard.global.ts`
- `app/pages/index.vue`
- `app/pages/login.vue`
- `app/components/AppFooterLeft.vue`
- `app/components/AppHeaderBottom.vue`
- `app/components/AppHeaderBottomRight.vue`

**Shared layer:**
- `shared/types/auth.d.ts`
- `shared/utils/protected-routes.ts`
- `shared/utils/protected-routes.spec.ts`

**Server layer:**
- `server/tsconfig.json`
- `server/utils/user-store.ts`
- `server/utils/auth-adapter.ts`
- `server/api/auth/login.post.ts`
- `server/api/auth/logout.post.ts`
- `server/middleware/content-guard.ts`

**Content — all 8 sections (`.navigation.yml` + stub `index.md` files):**
- All listed in Phase 2 below (44 files)

**Assets:**
- `public/logo/favicon.png`
- `public/logo/logo-dark.png`
- `public/logo/logo-light.png`
- `public/strata-visual.html`

---

## 3. Migration Action Plan

Execute these steps in order before Phase 0 begins.

### Step M-1 — Delete extraneous files

> These are destructive actions. Confirm with the human engineer before proceeding.

```
DELETE: app/app.vue
DELETE: app/pages/[...slug].vue
DELETE: app/components/Alert.vue
DELETE: app/components/Counter.vue
DELETE: content/about.md
```

After deletion, the `app/components/` directory will be empty (correct — will be populated by Docus overrides in Phase 1). The `app/pages/` directory will be empty (correct — `index.vue` and `login.vue` will be created in Phases 1 and 3).

### Step M-2 — Replace `package.json`

Replace with the spec §7.1 version adapted for STRATA:

```json
{
  "name": "strataprotocol-org",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "nuxt dev",
    "build": "nuxt build",
    "preview": "nuxt preview",
    "type-check": "node scripts/type-check.mjs",
    "test:unit": "vitest"
  },
  "dependencies": {
    "bcryptjs": "^3.0.3",
    "better-sqlite3": "^12.2.0",
    "docus": "latest",
    "nuxt": "^4.3.1",
    "nuxt-auth-utils": "^0.5.29"
  },
  "devDependencies": {
    "@types/bcryptjs": "^2.4.6",
    "vitest": "^4.1.6",
    "vue-tsc": "^3.2.9"
  }
}
```

**Critical:** `generate` is NOT in scripts. `@nuxt/content` and `@nuxt/ui` are NOT in dependencies — they are provided by Docus. `better-sqlite3` is in `dependencies` (runtime, not dev — Nuxt Content uses it for its search index database).

### Step M-3 — Replace `nuxt.config.ts`

Replace with the spec §7.3 version adapted for STRATA:

```typescript
// nuxt.config.ts
export default defineNuxtConfig({
  extends: ['docus'],
  modules: ['nuxt-auth-utils'],
  site: {
    name: 'STRATA Protocol',
  },
  app: {
    head: {
      link: [
        { rel: 'icon', type: 'image/png', href: '/logo/favicon.png' },
      ],
    },
  },
  runtimeConfig: {
    // MVP user store: JSON array of user records with bcrypt-hashed passwords.
    // Set via NUXT_USERS_JSON environment variable — never commit real values.
    usersJson: '',
    session: {
      maxAge: 60 * 60 * 24 * 7,  // 7 days
    },
  },
  nitro: {
    // SECURITY: This site MUST run as a Node.js server (SSR).
    // `nuxt generate` (SSG) bypasses server/middleware/content-guard.ts entirely,
    // exposing restricted content to anyone. Never add a "generate" script.
    preset: 'node-server',
  },
  hooks: {
    // Fix Windows dev-server bug: Docus registers docus.css with an absolute Windows
    // path. Vite cannot resolve drive-letter paths as valid URLs, causing a 404.
    // Replacing with the #build alias resolves correctly.
    // This hook is harmless on Linux/macOS (the condition is never true).
    ready(nuxt) {
      nuxt.options.css = nuxt.options.css.map((css) => {
        if (typeof css !== 'string' || !css.includes('docus.css')) return css
        return '#build/docus.css'
      })
    },
  },
})
```

### Step M-4 — Replace `content.config.ts`

Replace with the spec §7.4 version (verbatim — no STRATA adaptations needed):

```typescript
// content.config.ts
import { defineContentConfig, defineCollection, z } from '@nuxt/content'

export default defineContentConfig({
  collections: {
    // Landing collection — only content/index.md (root home page).
    landing: defineCollection({
      type: 'page',
      source: {
        include: 'index.md',
      },
    }),

    docs: defineCollection({
      type: 'page',
      source: [
        {
          include: '**/*.md',
          exclude: ['index.md'],  // root index.md belongs to the landing collection
        },
        {
          include: '**/.navigation.yml',
        },
      ],
      schema: z.object({
        requiredRole: z
          .enum(['public', 'customer', 'partner', 'admin'])
          .optional()
          .default('public'),
        navigation: z
          .object({
            order: z.number().optional(),
            icon: z.string().optional(),
            exclude: z.boolean().optional(),
          })
          .optional(),
        hero: z
          .object({
            name: z.string().optional(),
            text: z.string().optional(),
            tagline: z.string().optional(),
            actions: z
              .array(
                z.object({
                  label: z.string(),
                  to: z.string(),
                  variant: z.string().optional(),
                })
              )
              .optional(),
          })
          .optional(),
        features: z
          .array(
            z.object({
              icon: z.string().optional(),
              title: z.string(),
              description: z.string(),
            })
          )
          .optional(),
      }),
    }),
  },
})
```

### Step M-5 — Verify `tsconfig.json`

The existing `tsconfig.json` has the correct Nuxt 4 references structure:
```json
{
  "files": [],
  "references": [
    { "path": "./.nuxt/tsconfig.app.json" },
    { "path": "./.nuxt/tsconfig.server.json" },
    { "path": "./.nuxt/tsconfig.shared.json" },
    { "path": "./.nuxt/tsconfig.node.json" }
  ]
}
```
This is correct per spec §7.5. No change needed.

### Step M-6 — Run `npm install`

After replacing `package.json`, run:
```bash
npm install
```
This installs `docus`, `nuxt-auth-utils`, `bcryptjs`, `better-sqlite3`, `vitest`, `vue-tsc`, and `@types/bcryptjs`.

### Step M-7 — Generate `.nuxt/` directory

```bash
npx nuxt dev --no-open
# Wait until the dev server starts and .nuxt/ is populated
# Then Ctrl+C to stop
```

**Why this is required:** `tsconfig.typecheck.json` (Phase 0) and `server/tsconfig.json` (Phase 3) extend files inside `.nuxt/`. These files do not exist until `nuxt dev` or `nuxt build` runs. The type checker will fail without them.

**Checkpoint M:** `.nuxt/` directory exists. `node_modules/` is populated. Continue to Phase 0.

---

## 4. Phase 0 — Configuration Files

Create these files after completing all migration steps:

### 4.1 `tsconfig.typecheck.json`

Per spec §7.6 (verbatim):

```json
{
  "extends": "./.nuxt/tsconfig.app.json",
  "include": [
    ".nuxt/nuxt.d.ts",
    ".nuxt/imports.d.ts",
    ".nuxt/components.d.ts",
    "app/**/*",
    "shared/**/*",
    "server/**/*"
  ],
  "exclude": ["node_modules", ".output"]
}
```

### 4.2 `vitest.config.ts`

Per spec §7.7 (verbatim):

```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    globals: true,
    environment: 'node',
    include: ['**/*.spec.ts'],
    exclude: ['node_modules', '.nuxt', '.output', '_bak', '_data', '_tmp'],
  },
})
```

### 4.3 `.env.example`

Per spec §7.8 (verbatim). No STRATA-specific changes. All operators must generate `NUXT_SESSION_PASSWORD` before any deployment.

**Checkpoint 0:** `npm run dev` starts without errors. The site loads at `http://localhost:3000` (may show minimal content — that's fine). `.nuxt/` directory exists.

---

## 5. Phase 1 — App Configuration

### Execution Order

Create files in this exact sequence:

1. `app/app.config.ts`
2. `app/app.css`
3. `app/types/auth.d.ts`
4. `app/composables/useSubNavigation.ts`
5. `app/pages/index.vue`
6. `app/components/AppFooterLeft.vue`
7. `app/components/AppHeaderBottom.vue`
8. `app/components/AppHeaderBottomRight.vue`

> **DO NOT** create `app/middleware/auth-guard.global.ts`, `app/pages/login.vue`, or any server/shared files in this phase. Those go in Phase 3.

### 5.1 `app/app.config.ts` — STRATA-Specific

Adapted from spec §8.1. The `ACTIVE_THEME` is set to `midnight`. All display names and URLs are STRATA-specific:

```typescript
// app/app.config.ts
const THEMES = {
  midnight: { primary: 'violet',  neutral: 'zinc'    },
  horizon:  { primary: 'cyan',    neutral: 'slate'   },
  forest:   { primary: 'emerald', neutral: 'neutral' },
  ocean:    { primary: 'blue',    neutral: 'slate'   },
  aurora:   { primary: 'purple',  neutral: 'gray'    },
} as const

const ACTIVE_THEME = 'midnight' as const

const theme = THEMES[ACTIVE_THEME]

export default defineAppConfig({
  seo: {
    title: 'STRATA Protocol — Governed AI-Assisted Development',
    description: 'The five-stratum framework for governed AI-assisted software delivery.',
  },
  header: {
    title: 'STRATA Protocol',
    logo: {
      light: '/logo/logo-dark.png',
      dark:  '/logo/logo-light.png',
      alt:   'STRATA Protocol',
    },
  },
  navigation: {
    sub: 'header',
  },
  github: {
    url: 'https://github.com/FlorantePascual/strata-protocol',
    branch: 'main',
  },
  toc: {
    title: 'On this page',
    bottom: {
      title: 'Resources',
      links: [
        {
          icon: 'i-simple-icons-github',
          label: 'GitHub',
          to: 'https://github.com/FlorantePascual/strata-protocol',
          target: '_blank',
        },
      ],
    },
  },
  ui: {
    colors: {
      primary: theme.primary,
      neutral: theme.neutral,
    },
  },
})
```

**Why `midnight`:** The violet/zinc theme conveys authority and structure — appropriate for a governance framework. Established in analysis.md §6.2 and confirmed in copilot-instructions.md §9.

### 5.2 `app/app.css`

Per spec §8.2 (verbatim).

### 5.3 `app/types/auth.d.ts`

Per spec §8.3 (verbatim). This augments `#auth-utils` User interface with RBAC fields for IDE tooling in `.vue` files. See also `shared/types/auth.d.ts` (Phase 3) which covers the server context.

### 5.4 `app/composables/useSubNavigation.ts`

Per spec §8.4 (verbatim). This override is REQUIRED — without it, navigation items appear in alphabetical order by filename stem, ignoring `navigation.order`. All eight section tabs would appear in wrong order.

### 5.5 `app/pages/index.vue`

Per spec §8.5 (verbatim). This override fixes gotcha G3 (stale null on client navigation back to `/`). Uses `queryCollection('landing')` with a function-based key.

### 5.6 `app/components/AppFooterLeft.vue` — STRATA-Specific

Adapted from spec §8.7:

```vue
<!-- app/components/AppFooterLeft.vue -->
<template>
  <div class="text-sm text-muted">
    Copyright © {{ new Date().getFullYear() }} StrataProtocol.org. All rights reserved.
  </div>
</template>
```

### 5.7 `app/components/AppHeaderBottom.vue`

Per spec §8.8 (verbatim). This override is REQUIRED to mount `AppHeaderBottomRight` (gotcha G12 — without this override, the auth controls never appear in the header).

### 5.8 `app/components/AppHeaderBottomRight.vue`

Per spec §8.9 (verbatim). Sign-in / sign-out controls with the two-step sign-out sequence.

**Checkpoint 1:** `npm run dev` starts without errors. The site loads at `http://localhost:3000`. The header shows the STRATA Protocol title and the violet theme primary color. The sign-in button appears in the header bar.

---

## 6. Phase 2 — STRATA Content Structure

This phase creates all content scaffold files. These are stub pages — they will be replaced with production content by REQ-001 through REQ-013. The stubs must have correct frontmatter (especially `requiredRole` and `navigation.order`) from day one.

### 6.1 STRATA Content File Tree

```
content/
├── index.md                              ← landing collection; STRATA homepage
│
├── documentation/
│   ├── .navigation.yml
│   ├── index.md                          ← public
│   ├── introduction/
│   │   └── index.md                      ← public
│   ├── getting-started/
│   │   └── index.md                      ← public
│   ├── the-strata/
│   │   ├── index.md                      ← public
│   │   ├── stratum-1-classification/
│   │   │   └── index.md                  ← public
│   │   ├── stratum-2-derivation/
│   │   │   └── index.md                  ← public
│   │   ├── stratum-3-authority-chain/
│   │   │   └── index.md                  ← public
│   │   ├── stratum-4-execution-loop/
│   │   │   └── index.md                  ← public
│   │   └── stratum-5-artifact-trail/
│   │       └── index.md                  ← public
│   ├── concepts/
│   │   └── index.md                      ← public
│   ├── reference/
│   │   ├── index.md                      ← public (key glossary terms)
│   │   └── full-glossary.md              ← customer (full STRATA glossary)
│   ├── deep-dives/
│   │   ├── index.md                      ← customer (stub → content by REQ-005)
│   │   ├── stratum-1-classification/
│   │   │   └── index.md                  ← customer stub
│   │   ├── stratum-2-derivation/
│   │   │   └── index.md                  ← customer stub
│   │   ├── stratum-3-authority-chain/
│   │   │   └── index.md                  ← customer stub
│   │   ├── stratum-4-execution-loop/
│   │   │   └── index.md                  ← customer stub
│   │   └── stratum-5-artifact-trail/
│   │       └── index.md                  ← customer stub
│   └── account/
│       └── index.md                      ← customer
│
├── developer-guide/
│   ├── .navigation.yml                   (title: Templates)
│   ├── index.md                          ← customer
│   ├── introduction/
│   │   └── index.md                      ← customer
│   ├── getting-started/
│   │   └── index.md                      ← customer
│   ├── guides/
│   │   ├── index.md                      ← customer
│   │   ├── nuxt-docus/
│   │   │   ├── index.md                  ← customer
│   │   │   ├── be-feature-template/
│   │   │   │   └── index.md              ← customer stub → content by REQ-006
│   │   │   └── fe-feature-template/
│   │   │       └── index.md              ← customer stub → content by REQ-006
│   │   └── nestjs-vue/
│   │       └── index.md                  ← customer (stub, content deferred)
│   ├── architecture/
│   │   └── index.md                      ← customer
│   └── reference/
│       └── index.md                      ← customer
│
├── case-studies/
│   ├── .navigation.yml
│   ├── index.md                          ← public stub → content by REQ-013
│   ├── strataprotocol-org/
│   │   └── index.md                      ← public stub
│   ├── fractal-osi/
│   │   └── index.md                      ← public stub
│   ├── one-verse-daily/
│   │   └── index.md                      ← public stub
│   └── ai9gm/
│       └── index.md                      ← public stub
│
├── ai-playbook/
│   ├── .navigation.yml
│   ├── index.md                          ← public
│   ├── introduction/
│   │   └── index.md                      ← public
│   ├── guides/
│   │   └── index.md                      ← customer stub → content by REQ-007
│   └── concepts/
│       └── index.md                      ← public
│
├── video-courses/
│   ├── .navigation.yml
│   └── index.md                          ← public stub (content deferred)
│
├── marketplace/
│   ├── .navigation.yml
│   └── index.md                          ← public stub
│
├── blog/
│   ├── .navigation.yml
│   └── index.md                          ← public stub
│
└── changelog/
    ├── .navigation.yml
    └── index.md                          ← public (initial commit entry)
```

**Total new files in Phase 2:** 48 files (8 navigation YMLs + 40 markdown files)

### 6.2 Navigation YML Files

All `.navigation.yml` files. Create exactly as specified — icons and orders are normative:

```yaml
# content/documentation/.navigation.yml
title: Documentation
icon: i-ph-book-open
navigation:
  order: 1
```

```yaml
# content/developer-guide/.navigation.yml
title: Templates
icon: i-ph-files
navigation:
  order: 2
```

```yaml
# content/case-studies/.navigation.yml
title: Case Studies
icon: i-ph-briefcase
navigation:
  order: 3
```

```yaml
# content/ai-playbook/.navigation.yml
title: AI Playbook
icon: i-ph-robot
navigation:
  order: 4
```

```yaml
# content/video-courses/.navigation.yml
title: Video Courses
icon: i-ph-video
navigation:
  order: 5
```

```yaml
# content/marketplace/.navigation.yml
title: Marketplace
icon: i-ph-storefront
navigation:
  order: 6
```

```yaml
# content/blog/.navigation.yml
title: Blog
icon: i-ph-newspaper
navigation:
  order: 7
```

```yaml
# content/changelog/.navigation.yml
title: Changelog
icon: i-ph-git-branch
navigation:
  order: 8
```

### 6.3 `content/index.md` — STRATA Homepage (Replace Existing)

This replaces the existing stub. Contains the STRATA hero, five-stratum feature grid, iframe for the architecture visualization, and the self-referential "Built on STRATA" section. Full content per `implementation-plan.md §6.3`:

```markdown
---
title: STRATA Protocol
description: The five-stratum framework for governed AI-assisted software delivery.
hero:
  name: "STRATA Protocol"
  text: "Governed AI-Assisted Development"
  tagline: "The framework that makes AI copilots accountable, traceable, and trustworthy — from classification to artifact trail."
  actions:
    - label: "Learn the Protocol"
      to: "/documentation"
      variant: "solid"
    - label: "Become a Member"
      to: "/login"
      variant: "outline"
features:
  - icon: "ph:stack"
    title: "Stratum 1 — Classification"
    description: "Classify before you specify. The project type determines documentation depth, risk tolerance, and AI session constraints."
  - icon: "ph:tree-structure"
    title: "Stratum 2 — Derivation"
    description: "Documents are derived, not written. Every artifact answers exactly one framing question."
  - icon: "ph:link-chain"
    title: "Stratum 3 — Authority Chain"
    description: "The AI is governed, not autonomous. The authority chain defines what the copilot may do and how it must think."
  - icon: "ph:arrows-clockwise"
    title: "Stratum 4 — Execution Loop"
    description: "The human is the gate. No phase advances without explicit sign-off. Each iteration feeds the next."
  - icon: "ph:archive"
    title: "Stratum 5 — Artifact Trail"
    description: "The artifact trail outlives the project. Every decision, deviation, and sign-off is documented and fed forward."
---

::u-page-hero
---
#title
STRATA Protocol

#description
**Governed AI-Assisted Development**

*Five load-bearing strata. Each one foundational to everything above it.*
---
  :::u-button
  ---
  color: neutral
  size: xl
  to: /documentation
  trailing-icon: i-lucide-arrow-right
  ---
  Learn the Protocol
  :::

  :::u-button
  ---
  color: neutral
  size: xl
  to: /login
  variant: soft
  ---
  Become a Member
  :::
::

::u-page-section
---
#title
What STRATA is

#description
STRATA is a software delivery methodology built for the era of AI-assisted development. It treats the AI copilot as a **governed actor** operating inside a defined mandate — not as a productivity shortcut.
---
  :::u-page-feature
  ---
  icon: i-ph-stack
  to: /documentation/the-strata/stratum-1-classification
  ---
  #title
  Stratum 1 — Classification

  #description
  Classify before you specify. The project type determines documentation depth, risk tolerance, and the constraints applied to every AI session downstream.
  :::

  :::u-page-feature
  ---
  icon: i-ph-tree-structure
  to: /documentation/the-strata/stratum-2-derivation
  ---
  #title
  Stratum 2 — Derivation

  #description
  Business intent cascades into technical artifacts. Documents are derived from framing questions, not written from instinct.
  :::

  :::u-page-feature
  ---
  icon: i-ph-link-chain
  to: /documentation/the-strata/stratum-3-authority-chain
  ---
  #title
  Stratum 3 — Authority Chain

  #description
  Every AI session is constitutionally bound. The copilot is a governed actor inside a hierarchy it cannot override.
  :::

  :::u-page-feature
  ---
  icon: i-ph-arrows-clockwise
  to: /documentation/the-strata/stratum-4-execution-loop
  ---
  #title
  Stratum 4 — Execution Loop

  #description
  Phased, per-requirement development. No phase advances without human sign-off. Deviations are documented and fed forward as context.
  :::

  :::u-page-feature
  ---
  icon: i-ph-archive
  to: /documentation/the-strata/stratum-5-artifact-trail
  ---
  #title
  Stratum 5 — Artifact Trail

  #description
  Every decision, deviation, and sign-off is documented. The trail outlives the project and makes every future decision informed.
  :::

  :::u-page-feature
  ---
  icon: i-ph-users
  to: /documentation/getting-started
  ---
  #title
  Start with Your Next Project

  #description
  STRATA is a practice, not a prerequisite. Begin with classification on your current project. The framework reveals its value in the first session.
  :::
::

::u-page-section
---
#title
The Architecture
---
<div class="my-8 rounded-xl overflow-hidden border border-default">
  <iframe
    src="/strata-visual.html"
    width="100%"
    height="600"
    frameborder="0"
    scrolling="no"
    title="STRATA Protocol Architecture Visualization"
    loading="lazy"
  ></iframe>
</div>
::

::u-page-section
---
#title
Built on STRATA

#description
This platform is the first project governed by the STRATA Protocol. Every architectural decision, every content derivation, and every phase of development followed the five strata. The artifact trail lives in the repository.
---
  :::u-button
  ---
  color: neutral
  size: xl
  to: /case-studies
  trailing-icon: i-lucide-arrow-right
  ---
  Read the Case Study
  :::
::
```

### 6.4 Documentation Section Stubs

Create each file with correct frontmatter. The content bodies are stubs — production content is authored per REQ-001 through REQ-005.

**Public pages (no `requiredRole`):**

```markdown
# content/documentation/index.md
---
title: Documentation
description: The complete STRATA Protocol framework — five strata for governed AI-assisted software delivery.
navigation:
  order: 1
---
> **Content stub.** Replace per REQ-001.
```

```markdown
# content/documentation/introduction/index.md
---
title: Introduction
description: What STRATA is, why it exists, and the governance problem it solves.
navigation:
  order: 1
---
> **Content stub.** Replace per REQ-001.
```

```markdown
# content/documentation/getting-started/index.md
---
title: Getting Started with STRATA
description: How to apply the STRATA Protocol on your first project — from classification to the first session.
navigation:
  order: 2
---
> **Content stub.** Replace per REQ-003.
```

```markdown
# content/documentation/the-strata/index.md
---
title: The Five Strata
description: The five load-bearing strata of the STRATA Protocol — an overview.
navigation:
  order: 3
---
> **Content stub.** Replace per REQ-001.
```

```markdown
# content/documentation/the-strata/stratum-1-classification/index.md
---
title: "Stratum 1 — Classification"
description: Classify before you specify. The project type determines every downstream engineering decision.
navigation:
  order: 1
---
> **Content stub.** Replace per REQ-001.
```

```markdown
# content/documentation/the-strata/stratum-2-derivation/index.md
---
title: "Stratum 2 — Derivation"
description: Documents are derived from business intent, not written from instinct. Each document answers exactly one framing question.
navigation:
  order: 2
---
> **Content stub.** Replace per REQ-001.
```

```markdown
# content/documentation/the-strata/stratum-3-authority-chain/index.md
---
title: "Stratum 3 — Authority Chain"
description: The five-node authority chain that constitutionally governs every AI session.
navigation:
  order: 3
---
> **Content stub.** Replace per REQ-001.
```

```markdown
# content/documentation/the-strata/stratum-4-execution-loop/index.md
---
title: "Stratum 4 — Execution Loop"
description: Phased, per-requirement development with human-gated phase advancement.
navigation:
  order: 4
---
> **Content stub.** Replace per REQ-001.
```

```markdown
# content/documentation/the-strata/stratum-5-artifact-trail/index.md
---
title: "Stratum 5 — Artifact Trail"
description: The artifact trail documents every decision, deviation, and sign-off — and outlives the project.
navigation:
  order: 5
---
> **Content stub.** Replace per REQ-001.
```

```markdown
# content/documentation/concepts/index.md
---
title: Core Concepts
description: The governing principles and mental models that run through all five strata.
navigation:
  order: 4
---
> **Content stub.** Replace per REQ-001.
```

```markdown
# content/documentation/reference/index.md
---
title: Reference
description: Key STRATA Protocol terms and canonical definitions.
navigation:
  order: 5
---
> **Content stub.** Replace per REQ-004.
```

**Member-only public-section pages:**

```markdown
# content/documentation/reference/full-glossary.md
---
title: Full Glossary
description: Complete glossary of all STRATA Protocol terms and definitions.
navigation:
  order: 1
requiredRole: "customer"
---
> **Content stub.** Replace per REQ-004. Member access required.
```

```markdown
# content/documentation/account/index.md
---
title: Account & Settings
description: Manage your STRATA Protocol membership, profile, and access settings.
navigation:
  order: 6
requiredRole: "customer"
---
> **Content stub.** Replace per REQ-005. Member access required.
```

**Deep-dives section (member — all pages):**

```markdown
# content/documentation/deep-dives/index.md
---
title: Deep Dives
description: Procedural deep-dive documentation for each of the five strata — member access required.
navigation:
  order: 7
requiredRole: "customer"
---
> **Content stub.** Replace per REQ-005. Member access required.
```

Create the following five with the same pattern (`requiredRole: "customer"`, numbered orders 1–5):

- `content/documentation/deep-dives/stratum-1-classification/index.md` — "Stratum 1 Deep Dive — Classification"
- `content/documentation/deep-dives/stratum-2-derivation/index.md` — "Stratum 2 Deep Dive — Derivation"
- `content/documentation/deep-dives/stratum-3-authority-chain/index.md` — "Stratum 3 Deep Dive — Authority Chain"
- `content/documentation/deep-dives/stratum-4-execution-loop/index.md` — "Stratum 4 Deep Dive — Execution Loop"
- `content/documentation/deep-dives/stratum-5-artifact-trail/index.md` — "Stratum 5 Deep Dive — Artifact Trail"

### 6.5 Developer Guide Section Stubs

All developer-guide pages are member-only (`requiredRole: "customer"`). The section title in nav is "Templates" — this comes from `.navigation.yml` (already specified in §6.2).

Stubs per the file tree in §6.1. Key frontmatter requirements:
- All pages: `requiredRole: "customer"`
- All pages: correct `navigation.order` (introduction: 1, getting-started: 2, guides: 3, architecture: 4, reference: 5)
- The `nuxt-docus/be-feature-template/index.md` and `nuxt-docus/fe-feature-template/index.md` stubs: these will be replaced by REQ-006

### 6.6 Case Studies Section Stubs

All case studies pages are public (no `requiredRole`). Stubs per §6.1. Production content by REQ-013.

### 6.7 AI Playbook Section Stubs

Mixed access:
- `ai-playbook/index.md` — public
- `ai-playbook/introduction/index.md` — public
- `ai-playbook/concepts/index.md` — public
- `ai-playbook/guides/index.md` — **`requiredRole: "customer"`** (member-only; production content by REQ-007)

### 6.8 Remaining Section Stubs

`video-courses/index.md`, `marketplace/index.md`, `blog/index.md` — all public stubs.

**`content/changelog/index.md`** — public but requires an ACTUAL initial commit entry (not just a stub), per the Vision & Business Case. Minimum content:

```markdown
---
title: Changelog
description: STRATA Protocol version history, updates, and methodology changes.
navigation:
  order: 1
---

# Changelog

## 2026-05-19 — Initial Commit (v0.1.0)

**STRATA Protocol platform — first public release.**

### What Was Built

- Complete Nuxt 4 / Docus documentation portal
- Five-stratum public documentation structure (stubs — full content in progress)
- Authentication system: secure session, invitation-based member provisioning
- Server-side content protection (dual-layer RBAC: browser guard + Content API guard)
- Case Studies section scaffolded (StrataProtocol.org as Case Study #1)
- Self-referential governance artifacts in `docs/strata/`

### Governance

This platform was built using the STRATA Protocol. The artifact trail is in `docs/strata/`.
Classification: Class 1.3 — Platformization.
```

**Checkpoint 2:** `npm run dev` — all eight section tabs appear in the header tab bar in the correct order (Documentation → Changelog). Sidebar shows section structure. No 404 errors on section landing pages. Clicking `/developer-guide` shows member stub content (no redirect yet — auth system not in Phase 2).

---

## 7. Phase 3 — Auth System

### Execution Order

Create files in this exact sequence:

1. `shared/utils/protected-routes.ts`
2. `shared/utils/protected-routes.spec.ts`
3. `shared/types/auth.d.ts`
4. `server/tsconfig.json`
5. `server/utils/user-store.ts`
6. `server/utils/auth-adapter.ts`
7. `server/api/auth/login.post.ts`
8. `server/api/auth/logout.post.ts`
9. `server/middleware/content-guard.ts`
10. `app/middleware/auth-guard.global.ts`
11. `app/pages/login.vue`

### 7.1 `shared/utils/protected-routes.ts` — STRATA-Specific

This is the STRATA adaptation of spec §9.1. The STRATA version adds `/documentation/deep-dives`, `/ai-playbook/guides`, and `/documentation/reference/full-glossary` to the DOCUS baseline, and removes the generic example comments. Entries are sorted most-specific-first per gotcha G10:

```typescript
// shared/utils/protected-routes.ts
export interface ProtectedRoute {
  prefix: string
  role: 'customer' | 'partner' | 'admin'
}

export const ROLE_HIERARCHY: Record<string, number> = {
  public: 0,
  customer: 1,
  partner: 2,
  admin: 3,
}

/**
 * Ordered list of protected path prefixes and their required roles.
 * IMPORTANT: Sort most-specific-first (longer prefixes before shorter).
 * Adding a new protected page requires:
 *   1. Add requiredRole to frontmatter
 *   2. Add entry here
 *   3. Add test case to protected-routes.spec.ts
 *   4. Run npm run test:unit -- --run
 */
export const PROTECTED_ROUTES: ProtectedRoute[] = [
  // Specific sub-page within a public section (most specific — listed first)
  { prefix: '/documentation/reference/full-glossary', role: 'customer' },

  // Sub-sections within public sections
  { prefix: '/documentation/deep-dives', role: 'customer' },
  { prefix: '/documentation/account', role: 'customer' },
  { prefix: '/ai-playbook/guides', role: 'customer' },

  // Entire sections — member-only
  { prefix: '/developer-guide', role: 'customer' },

  // Reserved — future partner tier
  { prefix: '/marketplace/premium', role: 'partner' },
]

export function findProtectedRoute(path: string): ProtectedRoute | null {
  return PROTECTED_ROUTES.find(r => path === r.prefix || path.startsWith(r.prefix + '/')) ?? null
}
```

### 7.2 `shared/utils/protected-routes.spec.ts` — STRATA 13 Test Cases

All 13 test cases required by REQ-010:

```typescript
// shared/utils/protected-routes.spec.ts
import { describe, it, expect } from 'vitest'
import { findProtectedRoute, ROLE_HIERARCHY } from './protected-routes'

describe('ROLE_HIERARCHY', () => {
  it('orders roles ascending from public to admin', () => {
    expect(ROLE_HIERARCHY['public']).toBeLessThan(ROLE_HIERARCHY['customer']!)
    expect(ROLE_HIERARCHY['customer']).toBeLessThan(ROLE_HIERARCHY['partner']!)
    expect(ROLE_HIERARCHY['partner']).toBeLessThan(ROLE_HIERARCHY['admin']!)
  })
})

describe('findProtectedRoute', () => {
  // ─── Public paths ──────────────────────────────────────────────────────────
  it('returns null for a public root path', () => {
    expect(findProtectedRoute('/documentation')).toBeNull()
  })

  it('returns null for a public sub-path', () => {
    expect(findProtectedRoute('/documentation/getting-started')).toBeNull()
  })

  it('returns null for the public /ai-playbook root', () => {
    expect(findProtectedRoute('/ai-playbook')).toBeNull()
  })

  it('does NOT partially match /documentation-extra (false prefix hit)', () => {
    expect(findProtectedRoute('/documentation-extra')).toBeNull()
  })

  // ─── Deep-dives ────────────────────────────────────────────────────────────
  it('matches /documentation/deep-dives exactly', () => {
    expect(findProtectedRoute('/documentation/deep-dives')?.role).toBe('customer')
  })

  it('matches a sub-path under /documentation/deep-dives', () => {
    expect(findProtectedRoute('/documentation/deep-dives/stratum-1-classification')?.role).toBe('customer')
  })

  // ─── Account ───────────────────────────────────────────────────────────────
  it('matches /documentation/account', () => {
    expect(findProtectedRoute('/documentation/account')).not.toBeNull()
  })

  // ─── Full glossary (sub-page protection) ───────────────────────────────────
  it('matches /documentation/reference/full-glossary exactly', () => {
    expect(findProtectedRoute('/documentation/reference/full-glossary')?.role).toBe('customer')
  })

  it('does NOT match /documentation/reference (public parent)', () => {
    expect(findProtectedRoute('/documentation/reference')).toBeNull()
  })

  // ─── AI Playbook guides ────────────────────────────────────────────────────
  it('matches /ai-playbook/guides exactly', () => {
    expect(findProtectedRoute('/ai-playbook/guides')?.role).toBe('customer')
  })

  it('matches a sub-path under /ai-playbook/guides', () => {
    expect(findProtectedRoute('/ai-playbook/guides/stratum-1-prompts')?.role).toBe('customer')
  })

  // ─── Developer guide ───────────────────────────────────────────────────────
  it('matches /developer-guide exactly', () => {
    expect(findProtectedRoute('/developer-guide')?.role).toBe('customer')
  })

  it('matches a sub-path under /developer-guide', () => {
    expect(findProtectedRoute('/developer-guide/guides/nuxt-docus/be-feature-template')?.role).toBe('customer')
  })

  // ─── Marketplace (partner tier) ────────────────────────────────────────────
  it('matches /marketplace/premium for partner role', () => {
    expect(findProtectedRoute('/marketplace/premium/some-plugin')?.role).toBe('partner')
  })
})
```

> This spec has 15 test cases (2 in `ROLE_HIERARCHY` + 13 in `findProtectedRoute`). Run with `npm run test:unit -- --run` to verify all pass.

### 7.3 `shared/types/auth.d.ts`

Per spec §9.3 (verbatim). Required for server-context type checking.

### 7.4 `server/tsconfig.json`

Per spec §10.1 (verbatim):
```json
{ "extends": "../.nuxt/tsconfig.server.json" }
```

### 7.5 `server/utils/user-store.ts`

Per spec §10.2 (verbatim). No STRATA-specific adaptations needed — the `StoredUser` interface is generic.

### 7.6 `server/utils/auth-adapter.ts`

Per spec §10.3 (verbatim). Provides `authenticate()` entry point dispatching to local (`NUXT_USERS_JSON`) or remote (`NUXT_AUTH_API_URL`) strategy.

### 7.7 `server/api/auth/login.post.ts`

Per spec §10.4 (verbatim). Server-side input validation + bcrypt authentication + `setUserSession()`.

### 7.8 `server/api/auth/logout.post.ts`

Per spec §10.5 (verbatim). Calls `clearUserSession(event)` and returns `{ ok: true }`.

### 7.9 `server/middleware/content-guard.ts`

Per spec §10.6 (verbatim). Guards `POST /__nuxt_content/*/query` endpoints. Uses `findProtectedRoute()` from `#shared/utils/protected-routes`.

### 7.10 `app/middleware/auth-guard.global.ts`

Per spec §8.6 (verbatim). Two-phase design: Phase A (unauthenticated fast-path using PROTECTED_ROUTES), Phase B (authenticated precise role check using queryCollection).

### 7.11 `app/pages/login.vue` — STRATA-Specific

Adapted from spec §8.10. Only the subtitle differs:

```
STRATA Protocol — members only area
```

The redirect sanitization logic, form validation, `fetchSession()` after login (gotcha G6), and all security constraints are verbatim from spec §8.10.

**Checkpoint 3:**
```bash
npm run test:unit -- --run   # all 15 spec tests pass
```
Manual:
- Navigate to `/developer-guide` without logging in → redirects to `/login?redirect=%2Fdeveloper-guide`
- Log in with a provisioned test user → redirects back to `/developer-guide`
- Navigate to `/documentation/deep-dives` without logging in → redirects to `/login`
- Navigate to `/ai-playbook/guides` without logging in → redirects to `/login`
- `POST /api/auth/logout` → returns `{ ok: true }`
- Direct `POST /__nuxt_content/docs/query` with protected path (unauthenticated) → HTTP 401

---

## 8. Phase 4 — CI Toolchain

### 8.1 `scripts/type-check.mjs`

Per spec §11.1 (verbatim). Dual-pass: `vue-tsc` (app context) + `tsc` (server context). Suppresses `node_modules` errors from Docus pre-existing strict-mode violations.

**Checkpoint 4:**
```bash
npm run type-check   # exit 0
npm run build        # exit 0
npm run test:unit -- --run   # exit 0
```

All three must pass before proceeding to Phase 5.

---

## 9. Phase 5 — Assets & Branding

### 9.1 `public/strata-visual.html` — Required

This is the cleaned `strata.html` artwork file for the homepage iframe. **This asset must exist for the homepage to render correctly.** The iframe at `/strata-visual.html` will show a broken frame until it is placed.

Preparation steps (from `implementation-plan.md §5.3`):

1. Open `docs/about/strata.html`
2. Remove all `@font-face` declarations referencing `https://assets.claude.ai/` URLs (Anthropic Sans) — these are external assets that may not be loadable in the site context
3. Remove any other external CDN or third-party URLs
4. Replace all font stacks with: `font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
5. Verify the visual renders acceptably in a browser with system fonts
6. Save as `public/strata-visual.html`

**Note:** If `docs/about/strata.html` does not yet contain or requires further cleanup, the homepage section can temporarily be hidden by commenting out the `::u-page-section` block containing the iframe. Do not ship a broken iframe.

### 9.2 Logo Files

Place in `public/logo/`:

| File | Description |
|---|---|
| `public/logo/favicon.png` | 32×32 or 64×64 PNG; STRATA mark |
| `public/logo/logo-dark.png` | Logo for use on light backgrounds (dark logotype) |
| `public/logo/logo-light.png` | Logo for use on dark backgrounds (light/white logotype) |

Until production logos are available, placeholder PNGs (solid color rectangles with text) are acceptable. The site will not error without them, but the header and browser tab will show broken images.

**Checkpoint 5 (Final):**

| Test | Expected |
|---|---|
| `npm run dev` | No errors; site loads |
| Root `/` | STRATA hero renders; nav shows 8 tabs in order |
| `/documentation` | Public documentation loads |
| `/developer-guide` (unauthenticated) | Redirects to `/login?redirect=%2Fdeveloper-guide` |
| `/documentation/deep-dives` (unauthenticated) | Redirects to `/login` |
| `/ai-playbook/guides` (unauthenticated) | Redirects to `/login` |
| `/login` | Login form renders with "STRATA Protocol — members only area" |
| Login (valid credentials) | Redirects to destination; header shows user name |
| Login (invalid credentials) | "Invalid email or password" — no stack trace |
| Sign out | Header reverts to "Sign in"; protected pages redirect |
| `POST /__nuxt_content/docs/query` (unauth, protected path) | HTTP 401 |
| `POST /__nuxt_content/docs/query` (auth, sufficient role) | HTTP 200 |
| `npm run type-check` | Exit 0 |
| `npm run build` | Exit 0 |
| `npm run test:unit -- --run` | Exit 0 |

---

## 10. Known Gotchas (Normative Summary)

These are verbatim from spec §15 — summarized here for quick reference during implementation:

| Code | Summary | Fix Location |
|---|---|---|
| G1 | Windows dev-server: Docus CSS 404 | `nuxt.config.ts` `hooks.ready` block — NEVER REMOVE |
| G2 | Navigation ordering is alphabetical by default (not by `order`) | `app/composables/useSubNavigation.ts` override required |
| G3 | Landing page stale null on client navigation | `app/pages/index.vue` function-based key |
| G4 | Landing collection must be separate from docs | `content.config.ts` two-collection setup |
| G5 | `getUserSession` must NOT be imported explicitly | Auto-import — no `import` statement |
| G6 | `fetchSession()` required after login before `navigateTo()` | `app/pages/login.vue` submit handler |
| G7 | `shared/types/auth.d.ts` required for server type checking | Both `app/types/auth.d.ts` AND `shared/types/auth.d.ts` must exist |
| G8 | Type-check needs two separate passes | `scripts/type-check.mjs` dual-pass |
| G9 | `readBody()` body caching in `content-guard.ts` | h3 caches body — safe to call twice |
| G10 | `PROTECTED_ROUTES` must be sorted most-specific-first | See `shared/utils/protected-routes.ts` §7.1 above |
| G11 | SSG is forbidden — always use `node-server` | `nuxt.config.ts` `nitro.preset` — NEVER CHANGE |
| G12 | `AppHeaderBottom.vue` override required for auth controls | `app/components/AppHeaderBottom.vue` must be created |

---

## 11. Canonical Decisions Established by REQ-000

| Decision | Value | Overrides |
|---|---|---|
| `/documentation/reference/full-glossary` in PROTECTED_ROUTES | **Yes — added** (security correction) | REQ-004's "frontmatter-only" decision (violates BR-RULE-01) |
| `content/documentation/deep-dives/` scaffolded in Phase 2 | **Yes — included** | DOCUS spec Phase 2 generic scaffold (which omitted it) |
| `content/documentation/introduction/` scaffolded in Phase 2 | **Yes — included** | REQ-001 which omitted explicit mention |
| Developer Guide section nav title | **"Templates"** | DOCUS generic "Developer Guide" |
| Theme | **`midnight` (violet/zinc)** | No prior conflict — confirmed from analysis.md |

---

## 12. Acceptance Criteria

REQ-000 is complete when all of the following pass:

1. **Migration complete:** All five files listed in Step M-1 have been deleted from the repository.
2. **Package integrity:** `package.json` has no `generate` script. `@nuxt/content` and `@nuxt/ui` are not in dependencies.
3. **`nuxt.config.ts` integrity:** `extends: ['docus']` is present. `nitro.preset: 'node-server'` is set. `hooks.ready` block is present.
4. **`content.config.ts` integrity:** Both `landing` and `docs` collections are defined. `index.md` is excluded from `docs`.
5. **All 48 content scaffold files exist** with correct frontmatter (`title`, `description`, `navigation.order`, `requiredRole` where applicable).
6. **`PROTECTED_ROUTES` has 6 entries** including `/documentation/reference/full-glossary`.
7. **`npm run test:unit -- --run` exits 0** with all 15 tests passing.
8. **`npm run type-check` exits 0** with no errors in project files.
9. **`npm run build` exits 0.**
10. **Manual auth verification passes** — all 5 protected routes redirect unauthenticated users; direct Content API calls to protected paths return 401.
11. **Homepage renders** with 8 nav tabs in correct order, violet theme, and STRATA hero text.
12. **`public/strata-visual.html` exists** and loads in the homepage iframe without errors.
13. **`.env` is in `.gitignore`.** `NUXT_SESSION_PASSWORD` and `NUXT_USERS_JSON` are never committed.

---

_End of REQ-000. This document is the pre-flight checklist for all subsequent REQ implementations. REQ-001 through REQ-013 may not begin until REQ-000 acceptance criteria are fully met._
