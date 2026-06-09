# Content File Structure Standard

**Standard ID:** CFS-001
**Status:** active
**Enforced since:** 2026-05-31
**Ratified:** 2026-05-31 — human review; REQ-005D authority source
**Full-tree compliance confirmed:** 2026-06-01 — Phase 3 verification; REQ-005D R-005/R-006
**Authority source:** REQ-005D Content Refactor — `docs/strata/4.execution-loop/sessions/REQ-005D-content-refactor/requirements-document.md`
**Aligned with:** `.github/copilot-instructions.md`, `CLAUDE.md`, `.github/instructions/content.instructions.md`, `.claude/rules/content.md`

---

Docus generates navigation automatically from the `content/` file tree and two metadata
sources: `.navigation.yml` files (folder-level) and `navigation` frontmatter (page-level).
Any structural inconsistency in `content/` surfaces directly as a navigation defect. This
standard defines the four rules that keep the tree correct, plus the pedagogical ordering
policy and canonical templates that govern every section folder.

---

## Rule 1 — Flat file over single-item folder

A section topic with **no sibling pages** must be a flat `.md` file, never a folder
containing only `index.md`. A single-item folder produces a redundant Nuxt Content group
node whose only child duplicates the group's name and path — the same title appears stacked
twice in the sidebar.

```
✅  content/developer-guide/introduction.md            ← flat file; correct
❌  content/developer-guide/introduction/index.md      ← single-item folder; defect D1
```

**Track-root exception:** Top-level track folders (`content/<track>/`) must remain folders
even when they contain only `index.md`, because they generate header navigation tabs.
Flattening a track root removes it from the tab bar.

```
✅  content/video-courses/             ← track root: keep as folder even with only index.md
    content/video-courses/index.md
```

In this repo, `content/marketplace/`, `content/updates/`, and `content/video-courses/` are
single-`index.md` track roots. They are **correct as folders** — flattening any of them would
drop it from the header tab bar. The single-item anti-pattern applies **only** to section
sub-folders inside a track, never to a track root.

---

## Rule 2 — Multi-item folder requires `.navigation.yml`

When a folder contains `index.md` **and** one or more sibling pages, it **must** include a
`.navigation.yml` that owns the folder-level `title`, `icon`, and `navigation.order`.

**Why:** Without `.navigation.yml`, Nuxt Content reads folder metadata from `index.md`,
forcing a single `navigation.order` value to control **both** the folder's position among
its parent's children **and** `index.md`'s position within the folder — a dual-control
conflict that makes correct ordering impossible (defect D2).

```
✅  content/documentation/the-strata/.navigation.yml         ← folder metadata (owns folder order)
    content/documentation/the-strata/index.md                ← section overview (pinned first)
    content/documentation/the-strata/stratum-1-classification.md  ← leaf page

❌  content/<track>/<section>/                               ← multi-item; no .navigation.yml (D2 defect)
    content/<track>/<section>/index.md                       ← forced to dual-control folder + self order
    content/<track>/<section>/some-leaf.md
```

**Repo status:** Every multi-item folder in this repo already carries a `.navigation.yml`, so
there is currently **no live D2 defect** — the ❌ case above is illustrative of the failure mode
the rule prevents. `content/documentation/the-strata/` and `content/documentation/deep-dives/`
are the canonical correct exemplars.

### `.navigation.yml` template

```yaml
title: The Five Strata              # human-readable section title — matches existing display title
icon: i-ph-stack                    # icon carried from the old index.md navigation.icon — keep verbatim
navigation:
  order: 3                          # folder position among its parent's children
```

**Migration note:** When adding `.navigation.yml` to a folder, move the `navigation.order`
value currently on the folder's `index.md` into `.navigation.yml`. Then reset `index.md` to
`order: 0` per Rule 3 below. Move the `icon` value from `index.md`'s `navigation` block
into `.navigation.yml` — it belongs at the folder level, not the page level.

---

## Rule 3 — `index.md` is pinned first with `order: 0`

Once a folder has a `.navigation.yml` (Rule 2), `index.md` is simply one sibling among the
folder's children. Set `navigation.order: 0` in `index.md` frontmatter to pin it first. The
folder's own position among its parent's children is governed exclusively by `.navigation.yml`.

```yaml
# content/documentation/the-strata/.navigation.yml
navigation:
  order: 3        # folder's position among documentation/ sections

# content/documentation/the-strata/index.md frontmatter
navigation:
  order: 0        # pinned first within the folder

# content/documentation/the-strata/stratum-1-classification.md frontmatter
navigation:
  order: 1        # first leaf page after the index
```

Remove the `icon` key from `index.md`'s `navigation` block after migrating it to
`.navigation.yml`. Competing folder icons cause unexpected rendering.

**D3 defects in this repo — resolved (2026-06-01):** four section `index.md` files carried
`navigation.order: 1` instead of `0`, each colliding with a sibling page also at `1`:
`content/ai-playbook/index.md`, `content/case-studies/index.md`,
`content/developer-guide/index.md`, and `content/documentation/index.md`. All reset to `order: 0`
in REQ-005D Phase 2 (R-001). Full-tree compliance confirmed in Phase 3.

---

## Rule 4 — No redundant body H1

Docus's `[...slug].vue` renders the frontmatter `title` via `<UPageHeader>` **and** the
full markdown body via `<ContentRenderer>`. A leading `# H1` in the body produces a
duplicate heading visible to every reader (defect D4).

**Correct approach:** Delete the leading body `# H1` where it duplicates (verbatim or
semantically) the frontmatter `title`. The frontmatter `title` remains the page heading;
the body begins with the introductory paragraph.

```markdown
❌  ---
    title: Architecture Overview
    ---

    # Architecture Overview     ← DELETE — duplicates frontmatter title

    This section is for contributors…

✅  ---
    title: Architecture Overview
    ---

    This section is for contributors…   ← body starts here; <UPageHeader> renders the title
```

**Safety checks:**
- Only remove an H1 that **duplicates** the frontmatter `title`. A body H1 with genuinely
  different content must be flagged for human review, not deleted automatically.
- Every page must have an explicit frontmatter `title` before its H1 is removed. Add
  `title` to frontmatter first where it is absent.
- Never remove `##`/`###` section headings — only the leading document-title H1 is in scope.
- Never apply to `content/index.md` (the root landing page belongs to the `landing`
  collection and is rendered by `app/pages/index.vue` — `<UPageHeader>` is not used there).

**D4 defects in this repo — resolved (2026-06-01):** thirteen pages opened with a body `# H1`
duplicating the frontmatter `title` —
`content/ai-playbook/index.md`,
`content/case-studies/index.md`, `content/case-studies/ai9gm.md`,
`content/case-studies/fractal-osi.md`, `content/case-studies/one-verse-daily.md`,
`content/case-studies/strataprotocol-org.md`,
`content/changelog/index.md`, `content/changelog/roadmap.md`,
`content/developer-guide/index.md`, `content/documentation/developer-toolkit.md`,
`content/marketplace/index.md`, `content/updates/index.md`, and `content/video-courses/index.md`.
All body H1s removed in REQ-005D Phase 2 (R-002). Full-tree compliance confirmed in Phase 3.

**Excluded — not defects:** the `# [Project Name] — …` lines inside
`content/documentation/deep-dives/stratum-{1,2,3}-*.md` are template literals inside fenced
` ```markdown ` code blocks. They do not duplicate any page title and are protected by the
safety check above. Do not remove them.

**Governance alignment (R-007):** Both operative constitutions
(`.github/copilot-instructions.md` and `CLAUDE.md`, "Standard page template / no body H1") note
that Docus renders the frontmatter `title` via `<UPageHeader>`, so content bodies must not open
with a `# H1` duplicating the title. The mirror content-rule files
(`.github/instructions/content.instructions.md` and `.claude/rules/content.md`) carry this as
Rule 4. This standard and the governance files now agree.

---

## Pedagogical ordering policy

All leaf pages within a section must carry an explicit `navigation.order` so the sidebar
sequence is pedagogical, not alphabetical (defect D3).

These pages are **teaching material**. Ordering is a learning-design decision: within every
section, pages must flow so a reader encounters concepts in the order that builds
understanding — foundational and orientation material first, then progressively more
advanced or specialized, with prerequisites always preceding the pages that depend on them.

### Ordering source (in priority)

1. **Taught reading path is authoritative.** Where a section's `index.md` presents a
   deliberate reading path (a numbered list or "Recommended Path" sequence), the leaf
   `navigation.order` values **must** match that path so the sidebar mirrors what the
   overview teaches.

2. **Reorder for pedagogical soundness.** Where the `index.md` order is not pedagogically
   sound (a prerequisite page appears after the page that needs it, or an advanced topic
   precedes the basics), reorder for learning flow and update the `index.md` prose list to
   match. Pedagogy overrides the incidental existing order.

3. **Derive from the learning hierarchy.** Where `index.md` implies no order, use the
   five-layer hierarchy (Introduction → Getting Started → Guides → Concepts → Reference)
   and, within a section, increasing difficulty and prerequisite chains
   (orientation/overview → core tasks → advanced/edge-case pages). The
   `content/documentation/the-strata/` folder is the exemplar: `index.md` at `0`, then
   `stratum-1…5` at `1…5`, mirroring the taught sequence exactly.

### Mechanics

- Number leaf pages `1, 2, 3, …` with `index.md` at `0`.
- Integers must be **unique** within a folder — no ties.
- Use contiguous integers (no gaps) so the taught sequence is unambiguous and future
  insertions are obvious.
- After reordering, the section `index.md`'s "Recommended Path" or links list must agree
  with the new sidebar order. A sidebar that contradicts the overview's stated path
  reintroduces D3-class confusion.

### Exception — date-ordered tracks

`content/updates/` and `content/changelog/` order chronologically by design — chronology
is their pedagogy. Retain the existing date-based ordering; apply only the Rule 4 H1 sweep.
Do not renumber a track that already sorts correctly.

---

## Canonical templates

### Template A — `.navigation.yml` (multi-item section folder)

```yaml
title: Guides                       # human-readable section title
icon: i-ph-compass                  # i-ph-* icon syntax used throughout this repo; carry verbatim
navigation:
  order: 3                          # folder position among its parent's children
```

Real exemplar: `content/developer-guide/guides/.navigation.yml` uses exactly this shape.

### Template B — Section `index.md` frontmatter (pinned first, no body H1)

```yaml
---
title: Guides Overview
description: One sentence describing what this section contains.
requiredRole: "customer"            # only if restricted — carry over unchanged
navigation:
  order: 0                          # pin first within the folder
---

This section contains task-based guides for…   ← body lede; NO "# Guides" H1
```

### Template C — Leaf page frontmatter (ordered, no body H1)

```yaml
---
title: Creating Instances on the Canvas
description: One sentence describing what the reader learns on this page.
navigation:
  order: 2                          # logical position among siblings (index.md is 0)
---

An **instance** is a concrete occurrence of a class…   ← lede first; NO duplicate H1
```

### Template D — Flat single-topic page (no siblings — Rule 1)

```yaml
---
title: Introduction
description: One sentence describing what the reader learns.
requiredRole: "customer"            # carry over unchanged
navigation:
  order: 1                          # position among developer-guide/ top-level pages
---

Body lede first; no body # H1.
```

---

## Compliance checklist

Run this against every section folder before marking a phase complete:

- [ ] **Rule 1** — If folder has only `index.md`: flat file used (exception: track roots remain folders)
- [ ] **Rule 2** — If folder has `index.md` + ≥1 sibling: `.navigation.yml` present
- [ ] **Rule 2** — `.navigation.yml` carries `title`, `icon`, and `navigation.order`
- [ ] **Rule 3** — `index.md` in the folder has `navigation.order: 0`
- [ ] **Rule 3** — `index.md` does **not** carry a `navigation.icon` (moved to `.navigation.yml`)
- [ ] **Rule 4** — No content page has a body `# H1` duplicating its frontmatter `title`
- [ ] **Rule 4** — Every content page has an explicit frontmatter `title`
- [ ] **Ordering** — Leaf `navigation.order` integers follow the taught reading path
- [ ] **Ordering** — Leaf `navigation.order` integers are unique within the folder (no ties)
- [ ] **Ordering** — Section `index.md` reading list agrees with the sidebar order

---

## References
- `content/documentation/deep-dives/.navigation.yml` — the existing correct exemplar in this repo
- Docus `node_modules/docus/app/pages/[[lang]]/[...slug].vue` — render contract that makes D4 a defect
