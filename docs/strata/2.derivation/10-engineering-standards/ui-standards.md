# UI Standards — StrataProtocol.org

**Stratum:** 3 — Authority Chain  
**Node:** Engineering Standards — 4a (UI)  
**Status:** Normative — v1.0  
**Applies to:** Nuxt 4 + Docus stack  
**Sourced from:** `reference-docs/engineering-standards-draft.md` (adapted for this stack)  
**Referenced by:** `.github/copilot-instructions.md` §9

---

## Overview

This document governs all UI construction decisions for StrataProtocol.org. It establishes the boundary between Docus's built-in capabilities, application-level overrides, and MDC content extensions.

**Governing principle: Layer-First, Custom-Second.** Use what Docus provides before overriding it. Override at the abstraction layer — never by modifying source packages.

> **Stack note:** This project uses `docus` (latest), which bundles `@nuxt/content` v3 and `@nuxt/ui`. Several patterns in the source draft (`tokens.config.ts`, `pinceau`, separate `@nuxt/content`/`@nuxt/ui` installs) apply to an older generation of Docus and are **not applicable here**. This document reflects the correct patterns for the current stack.

---

## 1. Directory Architecture

Three root-level layers, strictly separated:

```
strata/
├── content/            PURE Markdown (MDC) — no Vue, no JS, no components
├── public/             Raw uncompiled static assets (logos, images, favicons)
├── nuxt.config.ts      System orchestrator
├── content.config.ts   Collection schema and Zod validation
└── app/                ALL Vue application code lives here
    ├── app.config.ts   Site branding, navigation metadata, theme colors
    ├── app.css         Tailwind directives and CSS overrides (keep minimal)
    ├── components/
    │   ├── content/    MDC components — auto-registered in Markdown
    │   └── docus/      Docus layout overrides (exact filename matching)
    ├── composables/    Reusable Vue composition logic
    ├── pages/          Vue route pages — only for non-content routes
    ├── middleware/     Client-side route guards
    └── types/          TypeScript augmentations (e.g., auth.d.ts)
```

**Separation rules:**

- `content/` contains ONLY `.md` files and `.navigation.yml` files — no Vue, no JS
- `public/` contains ONLY static binary assets — no application code
- All Vue development (components, composables, pages, middleware) lives inside `app/`
- `app/pages/` is for non-content routes only (`login.vue`, custom 404, etc.)

---

## 2. Docus Component Override Pattern

To replace or extend Docus's built-in structural components (header, footer, search, navigation), use the Nuxt Layer system — **never fork or patch the upstream `docus` package**.

**How it works:** Docus uses Nuxt's component resolution priority. Any file in `app/components/docus/` whose name matches a Docus core component silently takes precedence.

**Rules:**

- Place overrides in `app/components/docus/`
- **The filename must match the Docus core component name exactly** (e.g., `AppFooter.vue`, `AppHeaderLogo.vue`, `AppFooterLeft.vue`)
- Preserve the internal HTML structural contracts — changing DOM structure can break Docus's responsive layout contracts and CSS targeting
- If you need new behavior, wrap or extend — do not replace the entire template unless absolutely necessary

**Currently overridden components in this project:**

| File | Purpose |
|---|---|
| `AppFooterLeft.vue` | STRATA branding in the footer left slot |
| `AppHeaderBottom.vue` | Sub-navigation row below the main header |
| `AppHeaderBottomRight.vue` | Right-side actions in the sub-navigation |

**Identifying overridable names:** Check `node_modules/docus/app/components/` for the canonical list of component names.

---

## 3. MDC Components for Content

Vue components placed in `app/components/content/` are **auto-registered globally** — they are available in any Markdown file via MDC syntax without explicit imports.

**When to create an MDC component:**

- Reusable rich UI elements embedded in documentation prose (callouts, comparison tables, live interactive demos)
- Elements that require reactive state, browser APIs, or computed output
- Wrappers that reduce repetitive MDC boilerplate across many pages

**When NOT to create an MDC component:**

- When a Docus/Nuxt UI built-in (`::card`, `::steps`, `::u-page-hero`, `::u-page-section`) covers the need — prefer built-ins
- When the element is used in exactly one file — inline MDC is sufficient
- When it belongs in the Docus override layer (`app/components/docus/`) — that is a different directory with different semantics

**Naming:** Use PascalCase for the file name. MDC syntax in Markdown uses the kebab-case equivalent:

```
app/components/content/StrataLayer.vue  →  ::strata-layer in Markdown
```

---

## 4. Theme Customization

All color and palette customization for this project goes through `app.config.ts` under the `ui` key. The active theme is `midnight` (primary: violet, neutral: zinc).

```typescript
// app/app.config.ts — theme section
export default defineAppConfig({
  ui: {
    colors: {
      primary: 'violet',   // changes the entire primary color scale
      neutral: 'zinc',     // changes the neutral / background scale
    },
  },
  // ...other config
})
```

**Rules:**

- Never hardcode color hex values in Vue templates or global CSS for theme properties — use Tailwind palette classes that reference the configured primary/neutral scales
- **Never install `pinceau` or create `tokens.config.ts`** — these are for Pinceau-based Docus (pre-v3) and are incompatible with this stack
- One-off component-level styling: use Tailwind utility classes or `<style scoped>` in the component
- Global one-off overrides: add to `app/app.css` using `@layer` or `@apply` — keep the file minimal

**Theme is switchable** — changing `primary` and `neutral` in `app.config.ts` is the only change required to retheme the entire site.

---

## 5. Content Collection Schema

Frontmatter validation is enforced via `content.config.ts` using Zod schemas. This prevents broken deployment states caused by missing frontmatter.

**Two collections — they must stay separate (see copilot-instructions §6 and §13):**

| Collection | Source | Rule |
|---|---|---|
| `landing` | `content/index.md` only | Separate Zod schema; never mixed with `docs` |
| `docs` | `content/**` excluding `index.md` | Main documentation schema |

**Required frontmatter for every `content/` page:**

```yaml
---
title: "Page Title"           # required; minimum 1 character
description: "One sentence."  # required
---
```

**Additional frontmatter for member-only pages (RBAC — see copilot-instructions §7):**

```yaml
requiredRole: "customer"   # or "partner" / "admin"
```

**Navigation ordering frontmatter (required for correct sidebar ordering):**

```yaml
navigation:
  order: 3    # integer; lower = higher in the sidebar
```

---

## 6. URL and Navigation Rules

### Content path → URL mapping

```
content/documentation/index.md            →  /documentation
content/case-studies/fractal-osi/index.md →  /case-studies/fractal-osi
content/blog/my-post.md                   →  /blog/my-post
```

**No numeric prefix ordering** — this project controls navigation order via `navigation.order` in `.navigation.yml` and page frontmatter, not via filename prefixes. Do not add `1.folder-name/` style prefixes.

### Navigation ordering

Navigation is sorted by the `order` field set in:

1. `.navigation.yml` at section level (controls top-level and group ordering)
2. `navigation.order` in page frontmatter (controls page ordering within a section)

Default Nuxt Content v3 sorts alphabetically by stem — this project overrides that via `app/composables/useSubNavigation.ts`. **Always set `navigation.order`** in new pages and sections.

### Draft / hidden files

Prefix a file with a dot (`.`) to exclude it from navigation and content parsing while keeping it in the repository:

```
content/blog/.draft-post.md   →  not served; not navigable
```

---

## 7. Critical Prohibitions (UI Layer)

These supplement the prohibitions in copilot-instructions §13:

1. **Never add `@nuxt/content` or `@nuxt/ui` as separate packages** — Docus bundles them; separate installs cause version conflicts that are hard to diagnose
2. **Never change `srcDir` in `nuxt.config.ts`** — it is `'app/'` and the entire layer resolution depends on it
3. **Never install `pinceau` or create `tokens.config.ts`** — incompatible with the current Docus version
4. **Never place Vue components inside `content/`** — content is pure MDC Markdown; all application code lives in `app/`
5. **Never create documentation pages in `app/pages/`** — use `content/` with MDC syntax; `.vue` pages are for login, error boundaries, and non-content routes only

---

_End of UI Standards v1.0_  
_Next review: After first UI customization requirement (post-initial-commit)._
