# StrataProtocol.org — Vision & Business Case

**Stratum:** 2 — Derivation, Document 01  
**Layer:** 1 — Why  
**Status:** Normative  
**Derived from:** Classification Record (1.3 — Platformization)  
**Signed:** 2026-05-18  

---

## Classification Reference

- **Class:** Class 1 — Startup / MVP Systems
- **Category:** 1.3 — Platformization
- **Record:** `docs/strata/1.classification/classification-record.md`

---

## The Problem

AI coding tools have achieved mainstream adoption. GitHub Copilot, Cursor, and Claude Code are used daily across teams of every size and maturity. The tools work. The problem is what happens when they are used without governance.

Without a governance framework, AI sessions operate against no mandate. They work with training defaults, not project-specific constraints. Generated code is fast but untraceable — decisions are implicit, not recorded. Architectural choices are made ad hoc by the AI, not by the engineer. Phases have no defined gates; "approval" is intuitive, not verified against criteria. The codebase accumulates complexity that cannot be explained because the reasoning was never recorded.

Experienced engineers compensate with personal discipline. Teams of mixed experience do not. The result is a pattern repeating across the industry: fast-looking development that produces fragile, unmaintainable, ungoverned systems. The AI copilot looks like a productivity multiplier. At the delivery level, it is often a technical debt accelerator.

**Problem statement:** Software teams adopting AI copilots lack a governance framework, producing code that is fast but unpredictable, untraceable, and difficult to maintain or hand off.

---

## The Opportunity

The AI coding tool market is at peak adoption velocity. Governance standards have not followed. No widely-adopted, complete framework exists for AI-governed software delivery. The practitioner community is aware of the governance gap and is improvising — custom instructions files, ad hoc templates, personal discipline — but has no canonical reference to converge on.

The STRATA Protocol addresses this gap with a complete, five-stratum governance framework covering the full delivery lifecycle: project classification, document derivation, authority chain establishment, governed execution loop, and artifact trail maintenance. The framework exists. The platform to deliver it does not.

The opportunity is timing: be the first canonical reference framework for AI-governed software delivery, during the adoption wave that has not yet produced a governance standard.

---

## The Vision

> _StrataProtocol.org becomes the first port of call for software teams beginning any AI-assisted project — the governance framework that makes AI copilots accountable, traceable, and trustworthy rather than fast and fragile._

---

## Business Objectives

| Objective | Measurement | MVP Target |
|---|---|---|
| Establish public presence | Site live, publicly accessible, indexed | Launch |
| Complete public documentation | All 5 strata publicly documented | Launch |
| Demonstrate self-referential credibility | Platform built using STRATA; artifact trail in `docs/strata/` | Launch |
| Validate member demand | First 10 registered members | Month 1 |
| Member toolkit usefulness | Templates used in at least 3 external projects | Month 3 |
| Community recognition | Referenced in at least 1 engineering publication or community | Month 6 |

---

## Scope Boundary

### In scope — MVP

- Public documentation of all five strata (high-level narratives + getting started)
- Visual representation of the STRATA architecture (strata.html visualization)
- Member authentication (login, session management, logout)
- Member-only content: deep-dive per-stratum documentation, BE and FE implementation templates, development prompts
- Glossary (public: key terms; member: full glossary)
- Self-referential artifact trail in `docs/strata/`
- Case Studies section (initially: StrataProtocol.org as Case Study #1)
- Blog and Changelog for methodology evolution

### Out of scope — MVP (future initiatives)

- Self-service member registration (MVP: invitation-based, manual provisioning)
- Community features (discussion forums, Q&A, member profiles)
- API for programmatic template access
- Admin dashboard for member management
- Video courses (section created but content deferred)
- Marketplace premium tier (section created but partner features deferred)
- Multi-language support
- Platform-as-a-Service tooling (automated STRATA session runners)
- Mobile application

---

## Success Criteria

The MVP is successful when:

1. All five strata are publicly documented with enough depth for a practitioner to understand the framework end-to-end without reading the source documents
2. A registered member can log in and immediately access and use at least one BE or FE implementation template
3. The artifact trail in `docs/strata/` demonstrates at minimum Strata 1, 2, and 3 artifacts for this project
4. The site passes a Google Core Web Vitals check (LCP < 2.5s, CLS < 0.1)
5. The authentication system correctly protects member content against unauthenticated API calls (not just browser-level protection)

---

## Strategic Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Framework perceived as theoretical, not practical | Medium | High | Self-referential proof-of-concept; template quality; getting-started guide |
| AI tool landscape shifts (major vendor releases governance tools) | Low | High | STRATA's strength is framework completeness, not tool integration |
| Member content too thin at launch | Medium | High | Ensure minimum viable template depth; don't launch without usable templates |
| Self-referential complexity confuses visitors | Low | Medium | Clear "About this site" content; homepage messaging separates framework from platform |
| Manual membership provisioning creates friction | Medium | Medium | Document process clearly; add self-service registration in Phase 2 |
