# StrataProtocol.org — Business Requirements Document (BRD)

**Stratum:** 2 — Derivation, Document 03  
**Layer:** 2 — What  
**Status:** Normative  
**Derived from:** Vision & Business Case (Document 01)  
**Signed:** 2026-05-18  

---

## Stakeholder Map

| Stakeholder | Role | What they need |
|---|---|---|
| **Public visitor** | Anonymous practitioner | Understand STRATA; evaluate whether to become a member |
| **Member** | Registered practitioner | Apply STRATA on real projects; access templates and prompts |
| **Platform operator** | Admin | Manage member access; publish and update content |
| **Future partner** | Organization embedding STRATA | Programmatic access to templates and framework materials |

---

## Business Rules

- **BR-RULE-01:** Content access must be enforced at both the browser level (route guard) and the server level (Content API guard). Browser-only protection is not acceptable.
- **BR-RULE-02:** Member credentials must never be stored as plain text. bcrypt hashing (cost factor 12) is required.
- **BR-RULE-03:** The platform must never be served as a static site (SSG). The content guard requires a running Node.js server.
- **BR-RULE-04:** The `docs/strata/` artifact trail is the internal source of truth and is **never served by the public web server**. Its content is **conditioned** — formatted for MDC presentation, annotated with navigation metadata, and stripped of internal-process-only notes — before being published to the corresponding sections in `content/`. The `content/` published version is what practitioners read; the `docs/strata/` version is the governed original. Both must remain coherent and in sync.
- **BR-RULE-05:** Every protected content route must be registered in both the frontmatter and the `PROTECTED_ROUTES` configuration. A frontmatter-only registration is a security gap.

---

## Business Requirements

### Public Documentation

**BR-001 — The platform must present the STRATA Protocol framework publicly**  
The five strata must be publicly accessible in a navigable documentation structure. A practitioner must be able to understand the complete framework without creating an account. High-level narratives for all five strata must be public.  
*Traces to objective: Establish public presence; Complete public documentation*

**BR-002 — The platform must visually represent STRATA's architecture**  
The homepage must include the STRATA visualization (the layered architecture diagram) without requiring login. This visualization is the primary spatial representation of the framework.  
*Traces to objective: Establish public presence*

**BR-003 — The platform must provide a getting-started guide for new practitioners**  
A practitioner arriving for the first time must be able to follow a clear, linear path from "I don't know what STRATA is" to "I know how to begin applying it on a project" without leaving the Documentation section.  
*Traces to objective: Complete public documentation*

**BR-004 — The platform must provide a public glossary of key STRATA terms**  
The foundational terms of the STRATA Protocol (stratum, artifact trail, authority chain, classification, derivation, etc.) must be publicly accessible. Members receive the complete glossary; public visitors receive the key terms.  
*Traces to objective: Complete public documentation*

### Member-Only Content

**BR-005 — The platform must provide deep-dive documentation for members**  
Registered members must have access to detailed, procedural documentation for each of the five strata — beyond the public narrative. This includes the per-stratum procedural guides, the document templates, and the challenge frameworks.  
*Traces to objective: Validate member demand; Member toolkit usefulness*

**BR-006 — The platform must provide implementation templates for members**  
Members must be able to download and use ready-to-use implementation templates for BE and FE development cycles. Templates must be functional immediately — not placeholder stubs.  
*Traces to objective: Member toolkit usefulness*

**BR-007 — The platform must provide development prompts for members**  
Members must have access to STRATA-specific prompts for each stratum phase. These prompts must be ready-to-paste into their AI coding tool of choice.  
*Traces to objective: Member toolkit usefulness*

### Authentication & Access Control

**BR-008 — The platform must authenticate members with a secure session**  
Login must produce a sealed, HttpOnly session cookie. Sessions must expire after 7 days. The session must include the user's role for RBAC enforcement.  
*Traces to business rule: BR-RULE-01, BR-RULE-02*

**BR-009 — Member provisioning for MVP is invitation-based (operator-managed)**  
For the MVP, member accounts are provisioned manually by the operator via `NUXT_USERS_JSON`. Self-service registration is out of scope for MVP.  
*Future requirement: Self-service registration with email verification*

**BR-010 — Protected content must be inaccessible to unauthenticated API calls**  
Direct HTTP calls to the Nuxt Content data API (`/__nuxt_content/`) must be blocked for protected routes, not just browser navigations. The server-side content guard enforces this independently of the browser route guard.  
*Traces to business rule: BR-RULE-01*

### Platform Quality

**BR-011 — The documentation experience must match best-in-class developer documentation**  
The site must provide: sidebar navigation, in-page table of contents, syntax-highlighted code blocks, search, dark/light mode, and responsive layout. These are provided by the Docus framework and must be preserved without degradation.  
*Traces to objective: Establish public presence*

**BR-012 — The platform must be deployable as a production Node.js server**  
The build output must be a `node .output/server/index.mjs` deployable. Docker is the target deployment container.  
*Traces to business rule: BR-RULE-03*

### Self-Reference

**BR-013 — The platform must serve as a living proof-of-concept that STRATA governs its own construction**  
The governance artifacts in `docs/strata/` must be conditioned and published to `content/` so that practitioners can navigate the STRATA Protocol's own artifact trail through the documentation site. The Case Studies section must document StrataProtocol.org as Case Study #1, and must include the three early-adoption case studies (Fractal OSI, One Verse Daily, AI9GM) as Cases #2–4, demonstrating diverse real-world application of STRATA principles before the formal specification existed.  
*Traces to objective: Demonstrate self-referential credibility*

---

## Addenda

This BRD is extended by numbered addenda. Addenda are additive and do not rescind BR-001…BR-013
except where they explicitly refine a requirement.

| Addendum | Title | Adds | Refines |
|---|---|---|---|
| **01** | [Monetization & Membership (BR-014)](business-requirements-addendum-BR-014-monetization.md) | BR-RULE-06…08; BR-014.1–.7 (graduated membership tiers, free registration, paid subscriptions, tiered gating, pricing/account surfaces) | BR-013 via **BR-RULE-08** (the self-referential proof remains public; only *added* proof depth may be gated) |

Governs: **REQ-014** (Product Area 6). The drafts `draft-BRD-monetization.md` and
`draft-BRD-access-matrix.md` are superseded by Addendum 01.

---

## Out-of-Scope Business Needs (Explicitly Listed)

The following were considered and explicitly deferred:

- Self-service registration and email verification
- Admin dashboard for member management
- Community features (comments, Q&A, member profiles)
- API for programmatic template access
- Video course content (structure only in MVP)
- Marketplace premium content (structure only in MVP)
- Multi-language support
- Native mobile app
