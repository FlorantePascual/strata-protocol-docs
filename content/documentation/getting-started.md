---
title: Getting Started with STRATA
description: How to apply the STRATA Protocol on your first project — from classification to the first session.
navigation:
  order: 2
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-05-24"
---

## The Problem

You are using GitHub Copilot, Cursor, or Claude to build software. The tools are fast — sometimes startlingly so. But over time, the codebase accumulates decisions you cannot trace: features the AI generated without full review, sessions that drifted beyond their intended scope, architecture choices that made sense in context but left no record explaining why.

The AI is working. But it is not governed.

This is not a capability problem. It is a governance problem. The tools do not know what kind of project they are operating on, what architectural constraints apply, or who is responsible for approving structural decisions. Each session starts fresh. Context evaporates between sessions. The codebase grows — but the reasoning behind it does not. Eventually, a future engineer — or a future AI session — inherits a system that works but cannot be safely extended, because no one can reconstruct why it was built the way it was.

## What STRATA Is

STRATA is a five-stratum protocol for governing AI-assisted software development. It gives you a structured sequence that ensures your AI tools operate against a defined mandate — derived from your actual project context — rather than their own training defaults.

The five strata are: **Classification** (deciding what kind of project this is before the first decision is made), **Derivation** (building the complete knowledge base in dependency order before writing any specification), **Authority Chain** (constitutionally mandating how your AI copilot operates on this project), **Execution Loop** (the human-gated cycle through which governed AI and engineer build software together, one phase at a time), and **Artifact Trail** (the living record that makes every decision durable beyond any single session).

STRATA is not an AI tool. It is not a prompt library. It is not a methodology like Agile or Scrum — it does not organize your sprints or your standups. It is an architectural governance protocol: a framework for ensuring that AI-generated work operates within defined constraints, at every phase, on every requirement.

::tip
The five strata are not a checklist. They are a sequential dependency chain — each stratum consumes the output of the one before it. Stratum 5 is only possible because Stratum 4 produced traceable artifacts, which is only possible because Stratum 3 established a governed mandate, which is only possible because Stratum 2 derived a complete knowledge base, which is only possible because Stratum 1 classified correctly.
::

## The Five Strata in 5 Minutes

::steps
### Stratum 1 — Classification :icon{name="i-ph-stack"}

Classification is the first act of technical authority: before any specification is written, you determine what kind of project this is by business intent, operational constraints, and continuity requirements. Getting classification right calibrates every downstream decision — architecture depth, document depth, and testing rigor — to the actual engineering situation, not a misread of it.

[Read the full narrative →](/documentation/the-strata/stratum-1-classification)

### Stratum 2 — Derivation :icon{name="i-ph-tree-structure"}

Derivation converts the Classification Record into a complete, challenged set of normative documents — produced in dependency order, each answering exactly one framing question. These documents are the knowledge base from which your authority chain and execution loop operate; a document that cannot be challenged against the ones above it in the dependency chain is not finalized — it is drafted.

[Read the full narrative →](/documentation/the-strata/stratum-2-derivation)

### Stratum 3 — Authority Chain :icon{name="i-ph-link-simple"}

The Authority Chain is a five-node constitutional framework derived from your Stratum 2 document set that mandates how your AI copilot operates on this specific project. It replaces the AI's training defaults with a project-specific mandate, converting the copilot from an autonomous guesser into a governed actor that escalates ambiguities rather than silently resolving them.

[Read the full narrative →](/documentation/the-strata/stratum-3-authority-chain)

### Stratum 4 — Execution Loop :icon{name="i-ph-arrows-clockwise"}

The Execution Loop is the structured, human-gated cycle through which a governed AI copilot and engineer build software together — one phase at a time, one requirement at a time. The human is the gate, not the reviewer: no phase advances without explicit sign-off, and each session opens against the authority chain and the current phase plan.

[Read the full narrative →](/documentation/the-strata/stratum-4-execution-loop)

### Stratum 5 — Artifact Trail :icon{name="i-ph-folder-open"}

The Artifact Trail is the continuously assembled record of every decision, deviation, gate pass, and context carried forward throughout the project. It does not replace the code — it encodes the *why* behind the code, making every architectural choice durable beyond any single session, any single engineer, or any single engagement.

[Read the full narrative →](/documentation/the-strata/stratum-5-artifact-trail)
::

## Applying STRATA on Your First Project

You do not need a complete implementation to begin. These three steps give you a governed starting point — all are completable today, without a member account.

::steps
### Step 1 — Classify your project

Before opening your first AI session, answer one question: what kind of project is this? A Greenfield MVP and a migration of an established production system require fundamentally different engineering strategies. Classifying correctly ensures that the documents you derive, the mandate you establish, and the code you generate are all calibrated to the actual situation — not a reasonable-but-wrong assumption about it.

Read [Stratum 1 — Classification](/documentation/the-strata/stratum-1-classification) to understand the classification dimensions and how to produce your first Classification Record.

### Step 2 — Set up your authority chain

Your authority chain is the document that governs your AI copilot on this project — derived from your classification and your project's normative documents, not written from instinct. It establishes scope, architecture constraints, escalation rules, and prohibited patterns before the first line of code is generated.

Read [Stratum 3 — Authority Chain](/documentation/the-strata/stratum-3-authority-chain) for the five-node structure and derivation sequence. The [AI Playbook](/ai-playbook) provides public guidance on deriving authority chains for the most common AI development tools.

### Step 3 — Create your artifact trail folder

Set up a `docs/strata/` directory at the root of your project. This is the canonical location for all STRATA-governed artifacts — your Classification Record, derivation documents, authority chain, session logs, and deviation records. The structure is five numbered folders, one per stratum:

```
docs/
  strata/
    1.classification/
    2.derivation/
    3.authority-chain/
    4.execution-loop/
    5.artifact-trail/
```

You do not need pre-filled templates to begin — create the folders, produce your Classification Record in `1.classification/`, and start from there. Member templates accelerate this step by providing pre-structured documents for each stratum, but the structure itself is public and sufficient for a first project.
::

## Next Steps

You have the map and the first three steps. Two paths are open from here.

Explore the complete documentation — everything below is publicly accessible, no account required:

::card-group
  ::card{title="Introduction" icon="i-ph-info" to="/documentation/introduction"}
  What STRATA is, why it exists, and the governance problem it was built to solve.
  ::
  ::card{title="The Five Strata" icon="i-ph-stack" to="/documentation/the-strata"}
  Full narrative pages for each stratum — from Classification to Artifact Trail.
  ::
  ::card{title="Core Concepts" icon="i-ph-lightbulb" to="/documentation/concepts"}
  The five governing principles that make the strata cohere across every project.
  ::
  ::card{title="Reference" icon="i-ph-list-bullets" to="/documentation/reference"}
  Glossary of STRATA terms and a reference index for the protocol's key concepts.
  ::
::

Or unlock the full implementation layer with a member account:

::u-page-c-t-a
---
title: Ready to go deeper?
description: Member accounts unlock the full implementation layer — deep-dives for each stratum, implementation templates for every phase, and AI prompts purpose-built for governed sessions.
variant: soft
links:
  - label: Create a member account
    to: /login
    icon: i-ph-user-plus
    color: neutral
---
::
