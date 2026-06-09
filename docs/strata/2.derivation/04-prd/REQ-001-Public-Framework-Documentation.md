# REQ-001 — Public Framework Documentation

**Stratum:** 2 — Derivation, Document 04  
**Layer:** 3 — How (Product Specification)  
**Status:** Normative  
**Traces to:** BR-001 — The platform must present the STRATA Protocol framework publicly  
**Derived from:** Business Requirements Document (Document 03)  
**Signed:** 2026-05-19  

---

## Summary

### Requirement Statement

The platform must present all five strata of the STRATA Protocol in a publicly navigable `/documentation/` section, with enough depth for any practitioner to understand the complete framework without creating an account.

### Business Need

The primary purpose of StrataProtocol.org is to make the STRATA Protocol accessible to practitioners worldwide. Without publicly visible framework documentation, no practitioner can evaluate whether STRATA addresses their governance gap, and no membership conversion can occur. The public documentation is the top of the funnel and the evidence of methodology credibility.

### Stakeholders

| Stakeholder | Need |
|---|---|
| **Public visitor** | Understand the framework, evaluate adoption, decide whether to become a member |
| **Member** | Use the same public documentation as a quick reference alongside member-only deep-dives |

---

## Scope

### In Scope

- The `/documentation/` section in its entirety (structure, navigation, public pages)
- All five stratum narrative pages (one per stratum, public access)
- The Documentation section landing page (`/documentation/`)
- Introduction sub-section (`/documentation/introduction/`)
- The Strata sub-section (`/documentation/the-strata/`) with all five child pages
- Concepts sub-section (`/documentation/concepts/`)
- Account sub-section scaffolding (`/documentation/account/`) — page exists, content is member-only (see REQ-005)
- Sidebar navigation and in-page table of contents for all documentation pages

### Out of Scope

- Deep-dive documentation (covered by REQ-005)
- Public glossary (covered by REQ-004)
- Getting-started guide (covered by REQ-003)
- Member authentication (covered by REQ-008)

---

## Product Specification

### Section Architecture

The `/documentation/` section is the primary content section of StrataProtocol.org. It is publicly accessible in its entirety except for `/documentation/account/` and `/documentation/deep-dives/`.

```
/documentation/
  ├── index.md                         (public) Framework overview landing
  ├── introduction/
  │   └── index.md                     (public) What STRATA is and why it exists
  ├── getting-started/
  │   └── index.md                     (public) → REQ-003
  ├── the-strata/
  │   ├── index.md                     (public) Strata overview — all five at a glance
  │   ├── stratum-1-classification/
  │   │   └── index.md                 (public) Stratum 1 public narrative
  │   ├── stratum-2-derivation/
  │   │   └── index.md                 (public) Stratum 2 public narrative
  │   ├── stratum-3-authority-chain/
  │   │   └── index.md                 (public) Stratum 3 public narrative
  │   ├── stratum-4-execution-loop/
  │   │   └── index.md                 (public) Stratum 4 public narrative
  │   └── stratum-5-artifact-trail/
  │       └── index.md                 (public) Stratum 5 public narrative
  ├── concepts/
  │   └── index.md                     (public) Governing principles, design philosophy
  ├── reference/
  │   └── index.md                     (public) → REQ-004 (Public Glossary)
  ├── deep-dives/
  │   └── (protected) → REQ-005
  └── account/
      └── index.md                     (customer) → REQ-005
```

### Navigation Configuration

The Documentation section must appear as the first item in the global sidebar navigation. Navigation ordering is controlled via `navigation.order` in frontmatter — NOT via filename sorting.

```yaml
# content/documentation/.navigation.yml
title: Documentation
icon: i-ph-book-open
navigation:
  order: 1
```

**Implementation note — Navigation Deduplication (Phase 0):** Nuxt Content v3 includes each directory's `index.md` both as the directory-level navigation node and as a duplicate child item. This was resolved in `app/composables/useSubNavigation.ts` (`sortNavigation()`) by filtering out children whose path equals the parent's path. The fix was applied as Phase 0 of this requirement, before any content authoring. No content files or `.navigation.yml` files were modified. See `docs/strata/4.execution-loop/sessions/REQ-001-Public-Framework-Documentation/analysis.md §2.6` for full details.

### Documentation Section Landing Page (`/documentation/index.md`)

The section landing page (`/documentation/`) must:
- State what the STRATA Protocol is in one sentence
- Present the five strata as navigable cards (using `::card-group` / `::card` MDC syntax)
- Link to the Introduction, Getting Started, and The Strata sub-sections
- Include a member CTA (not intrusive — a clear "Go deeper with a member account" section at the bottom)

### Introduction Page (`/documentation/introduction/index.md`)

Must contain:
- The problem STRATA solves (the governance gap in AI-assisted development)
- What STRATA is — a five-stratum governance framework
- Who STRATA is for (practitioners, tech leads, founders, architects)
- The self-referential proof: StrataProtocol.org is built using STRATA
- Link to Getting Started and The Strata sub-sections

### The Strata Overview Page (`/documentation/the-strata/index.md`)

Must contain:
- A visual or table overview of all five strata with their names, layers, and purpose
- Navigation links to each individual stratum page
- A framing statement about how the five strata interlock

### Individual Stratum Narrative Pages

Each of the five stratum narrative pages (`/documentation/the-strata/{stratum-name}/index.md`) must contain:

| Section | Required | Description |
|---|---|---|
| Name and definition | Yes | Stratum name + one-sentence definition |
| Why this stratum exists | Yes | The problem it solves in the governance model |
| Key concepts | Yes | Minimum 3 key concepts with brief definitions |
| How practitioners apply it | Yes | Step-by-step or principle-based guidance |
| What goes wrong without it | Yes | The failure mode if this stratum is skipped |
| Example (from StrataProtocol.org) | Yes | A concrete example drawn from this project's artifact trail |
| Deep-dive CTA (member only) | Yes | Invitation to access the procedural deep-dive with a member account |
| Related strata | Yes | Links to strata above and below in the hierarchy |

The five strata and their canonical names:

| Stratum | Name | Icon |
|---|---|---|
| 1 | Classification | `i-ph-stack` |
| 2 | Derivation | `i-ph-tree-structure` |
| 3 | Authority Chain | `i-ph-link-chain` |
| 4 | Execution Loop | `i-ph-arrows-clockwise` |
| 5 | Artifact Trail | `i-ph-archive` |

### Concepts Page (`/documentation/concepts/index.md`)

Must contain:
- Governing principles of the STRATA framework (minimum 5 principles)
- Each principle: name, statement, implication for practitioners
- Required principles to document:
  - **Derive, don't invent** — every document traces to a higher-layer intent
  - **Human as the gate** — no phase advances without explicit sign-off
  - **Governance by layer** — each stratum has a single framing question
  - **Traceability by design** — every decision is recorded in the artifact trail
  - **Self-referential proof** — the platform is its own first case study

---

## Content Authoring Constraints

All content files follow these rules (enforced by authority chain):

- File type: `.md` only — never `.vue` for documentation content
- Location: `content/documentation/` hierarchy
- Frontmatter required on every file:
  ```yaml
  ---
  title: "Page Title"
  description: "One descriptive sentence."
  navigation:
    order: N
  ---
  ```
- No `requiredRole` on any public page
- MDC syntax for rich components (cards, heroes, callouts)
- No inline JavaScript or Vue component code in content files

---

## Acceptance Criteria

1. All five stratum narrative pages (`/documentation/the-strata/{stratum}/`) are accessible without authentication — the server returns HTTP 200 for unauthenticated requests.

2. No page in `/documentation/` (except `/documentation/account/` and `/documentation/deep-dives/`) returns HTTP 401 or redirects to `/login`.

3. Each individual stratum page contains all required sections listed in the table above.

4. The Documentation section link is visible in the global sidebar navigation for all visitors (authenticated and unauthenticated).

5. The Documentation section landing page contains navigation cards for all five strata.

6. The sidebar displays the Documentation section tree including Introduction, The Strata (with all five child items), Concepts, and Reference.

7. No dead links (404) exist within the public Documentation section.

8. All pages render correctly on mobile (< 768px) and desktop (> 1024px) viewport widths.

9. Each stratum page includes a visible "deep-dive" CTA that links to `/login` (or to the deep-dive page for authenticated members).

10. The page titles and descriptions set in frontmatter render correctly in the browser `<title>` tag and meta description.

---

## Technical Constraints

- **Content location:** `content/documentation/` — all files are `.md`
- **No `.vue` pages for documentation** — creating `app/pages/documentation/` is prohibited
- **Navigation ordering:** `navigation.order` in frontmatter controls sidebar order, not filename sort
- **No separate `@nuxt/content`** — Docus bundles it; adding separately causes conflicts
- **The `landing` collection** — `content/index.md` is separate from the `docs` collection; the Documentation section feeds the `docs` collection (defined in `content.config.ts`)
- **MDC components available** without import: `::card`, `::card-group`, `::steps`, `::u-page-hero`, `::u-page-feature`, `::callout`

---

## Dependencies

| Dependency | Reason |
|---|---|
| REQ-011 — Documentation Platform Quality | Docus framework provides the sidebar, TOC, and search that make this section navigable |
| REQ-004 — Public Glossary | The `/documentation/reference/` page is part of this section's navigation tree |
| REQ-003 — Getting-Started Guide | The `/documentation/getting-started/` page is part of this section's navigation tree |

---

## Open Questions

None. This requirement is fully derived from BR-001, the classification record (Class 1.3 Platformization), and the content scaffold defined in `docs/strata/0.initial-commit/implementation-plan.md §6.1`.
