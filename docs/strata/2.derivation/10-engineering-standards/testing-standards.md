# Testing Standards — StrataProtocol.org

**Stratum:** 3 — Authority Chain  
**Node:** Engineering Standards — 4b (Testing)  
**Status:** Normative — v1.0  
**Applies to:** All TypeScript, Vue, and content source in this repository  
**Sourced from:** `reference-docs/testing-standards-draft.md` (adapted for this stack)  
**Referenced by:** `.github/copilot-instructions.md` §10

---

## Overview

These standards define the testing conventions for StrataProtocol.org. Goals:

- Prevent test sprawl and `__tests__/` dumping grounds
- Keep tests discoverable and co-located with the code they validate
- Ensure AI coding assistants and human engineers produce consistent, maintainable tests
- Maintain a passing test suite as a required gate for every phase delivery

---

## 1. Co-Location Rule (Canonical)

**Tests MUST live beside the file they test.** This is the single most important convention.

```
✅ Correct

shared/utils/protected-routes.ts
shared/utils/protected-routes.spec.ts

server/utils/session-validator.ts
server/utils/session-validator.spec.ts

app/composables/useSubNavigation.ts
app/composables/useSubNavigation.spec.ts
```

```
❌ Forbidden

__tests__/
shared/utils/__tests__/protected-routes.test.ts
tests/composables/useSubNavigation.test.ts
```

**Why co-location:**

- Ownership is obvious — the test is beside the code it owns
- Refactors stay atomic — moving a file moves its test
- Dead tests are visible — an orphaned spec is easy to spot
- Discoverability is instant — no hunting through a parallel tree
- AI assistants default to the correct location

---

## 2. Naming Convention

| Rule | Value |
|---|---|
| Extension | `*.spec.ts` only |
| Base name | Must match the source file exactly |
| No qualifiers | Never `*.test.ts`, `*.spec.final.ts`, `spec2.ts` |

```
✅  protected-routes.spec.ts
✅  useSubNavigation.spec.ts

❌  protected-routes.test.ts
❌  protected-routes.spec.new.ts
❌  new-test.ts
```

---

## 3. What Requires Tests

| Code | Required | Notes |
|---|---|---|
| Every function in `shared/utils/` | ✅ Required | Most testable units; pure functions; no framework dependencies |
| Every new `PROTECTED_ROUTES` entry | ✅ Required | Add a case in `protected-routes.spec.ts` and run `test:unit` |
| Server utilities with business logic | ✅ Required | `server/utils/*.ts` |
| Server API route handlers (complex) | ⚠️ Recommended | When branching logic or auth decisions are present |
| Vue composables with logic | ⚠️ Recommended | Co-located `.spec.ts`; skip if the composable is purely reactive wiring |
| Vue components with non-trivial logic | ⚠️ Case-by-case | Only when the component has testable computed or conditional logic |
| Navigation config / `.navigation.yml` | ⚠️ Integration | Validate via `useSubNavigation` unit tests |
| Content `.md` files | Optional | Frontmatter schema compliance if regressions are a concern |

---

## 4. Test Types

### Unit Tests (primary)

Focus on isolated logic. Avoid rendering, network calls, and filesystem access.

- **Location:** Co-located beside the source file
- **Scope:** Functions, utilities, composables with logic, server utilities
- **Framework:** Vitest (configured in `vitest.config.ts`)
- **Command:** `npm run test:unit -- --run`

### Integration Tests

Validate interactions between units — content pipelines, navigation generation, middleware logic, schema validation.

- **Location:** Co-located near the relevant domain code (same naming rule applies)
- **Scope:** Server middleware logic, content schema validation, navigation composable behavior with real data shapes
- **Same co-location and `*.spec.ts` naming convention applies**

### E2E Tests

**Deferred to post-MVP** — not part of the initial commit scope.

When introduced, E2E tests will live in `test/e2e/` and use Playwright. Approved E2E scope:

- Login / logout cycle
- Protected route redirect behavior
- Navigation integrity (all nav items resolve without 404)
- Search flow (when search is enabled)
- Content access gates (member-only content returns 403 without auth)

E2E tests MUST NOT duplicate unit test coverage or validate internal implementation details.

---

## 5. Assertion Style

**Prefer semantic assertions over snapshots.**

```typescript
// ✅ Preferred — intent is clear, brittle to nothing except the value
expect(route.role).toBe('customer')
expect(PROTECTED_ROUTES).toHaveLength(5)
expect(session.user.email).toBeDefined()
expect(renderedHeading).toContain('Getting Started')

// ⚠️ Acceptable only for small, intentional, high-signal snapshots
expect(smallRenderedChunk).toMatchSnapshot()
```

**Snapshot usage rules:**

- Snapshots MUST be minimal — no entire-page or large HTML blob snapshots
- Snapshots MUST be intentional — the reviewer must be able to tell at a glance whether the snapshot diff is correct
- If a snapshot exceeds ~20 lines, reconsider: replace with targeted assertions
- Snapshot sprawl produces noisy code reviews, encourages blind approvals, and becomes brittle during content iteration

---

## 6. The `test/` Root Directory

A root-level `test/` directory MAY exist, but ONLY for infrastructure that cannot live next to code.

**Allowed:**

```
test/
  e2e/           Playwright specs (post-MVP)
  fixtures/      Shared test data / mock content files
  setup/         Vitest global setup files
  helpers/       Shared test utilities
```

**Forbidden:**

```
test/
  components/    ← must be co-located
  composables/   ← must be co-located
  utils/         ← must be co-located
```

The `test/` directory MUST NOT become an alternative source tree.

---

## 7. AI Assistant Rules

AI-generated tests MUST follow all of the above. Specifically:

- MUST co-locate tests beside the target file
- MUST use `*.spec.ts` naming
- MUST NOT create `__tests__/` directories anywhere in the project
- MUST NOT introduce parallel testing directory structures
- MUST prefer semantic assertions over `toMatchSnapshot()`
- MUST run `npm run test:unit -- --run` after generating test files and confirm exit 0 before completing the session

---

## 8. Approved Tooling

| Purpose | Tool | Status |
|---|---|---|
| Unit + integration tests | Vitest ^4.0 | Active |
| E2E tests | Playwright | Deferred — post-MVP |
| Component testing utilities | @testing-library/vue | Optional — if needed |

**Standard test command:**

```bash
npm run test:unit -- --run   # one-shot run; exit code 0 = all pass
```

**Full CI sequence (DOCUS Phase 4 gate):**

```bash
npm run type-check && npm run build && npm run test:unit -- --run
```

---

## 9. CI Requirements (DOCUS Phase 4)

When the CI/CD pipeline is configured (DOCUS Phase 4):

- Unit tests run on every PR and must exit 0
- `npm run type-check` runs in the same job
- `npm run build` runs in the same job
- E2E tests run in a separate job when introduced (post-MVP)
- Snapshot diffs that are not explicitly approved block merge

---

_End of Testing Standards v1.0_  
_Next review: When E2E tests are introduced (post-MVP)._
