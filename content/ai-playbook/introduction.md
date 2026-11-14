---
title: Introduction
description: What the AI Playbook covers — how to work with AI copilots inside the STRATA governance framework.
navigation:
  order: 1
authors:
  - name: "Florante Pascual"
    url: "https://florantepascual.com"
    avatar: "https://strataprotocol.org/lib/author/florante-pascual.png"
date: "2026-06-03"
---

Most practitioners use AI coding tools as general assistants: ask a question, accept the best answer, move on. STRATA treats AI tools differently — as governed agents with a defined mandate, operating within a constraint boundary established by the project's authority chain. The AI Playbook explains how that governance model works and provides the ready-to-use prompts that make it practical.

## Why Governance Is Not the Same as Prompt Engineering

Prompt engineering asks: "How do I get better output from this AI?" Governance asks: "What output is this AI authorized to produce?" Both questions matter, but they operate at different levels.

A well-crafted prompt produces useful output. A governed prompt produces useful output that is bounded, traceable, and consistent with the decisions made upstream. The difference becomes visible when something goes wrong: a project with only prompt engineering has no mechanism for identifying why the AI went in an unexpected direction. A project with governance has a session mandate that defines the boundary, and a deviation record that captures where the AI crossed it.

STRATA does not replace prompt engineering. It adds the constraint layer that makes AI-assisted work defensible over time — across multiple sessions, multiple requirements, and multiple practitioners.

## The Three Roles of AI in STRATA

AI tools in a STRATA-governed project operate in three distinct roles depending on what is being asked of them.

**Responder.** The AI receives a bounded request and produces an answer within the session mandate. This is the most common mode: a practitioner asks the AI to explain a behavior, evaluate an approach, or validate code against a specification. Responder mode is constrained by the operative constitution and the active phase plan.

**Generator.** The AI produces a new artifact — a Classification Record, a BRD section, a code module, a phase plan — from a specification. Generator mode requires explicit context: the upstream document that authorizes the generation must be present in the session. Without it, the AI generates plausibly useful output that is not traceable to the governance record.

**Documenter.** The AI records what was decided and why. In STRATA, governance is maintained through the artifact trail — the chain of documents from Classification through Execution. The Documenter role is invoked whenever a decision, deviation, or escalation needs to be captured: producing a deviation record, updating a phase notes file, or conditioning a governance document for publication.

## How the Authority Chain Constrains AI Behavior

At the session level, the authority chain manifests as the session mandate: the set of constraints the AI operates under for the duration of one chat session. The mandate specifies which requirement the AI is working on, which phase plan it must follow, which deviations require a log entry, and which conditions trigger a pause for human review.

A properly established session mandate means the AI cannot expand scope without explicit instruction, propose architectural changes that were not in the phase plan, or treat its own suggestions as authoritative. The authority chain answers the question the AI cannot answer for itself: "What am I authorized to do here?"

::tip
The session mandate is what brings the authority chain into a live AI session. Without it, the AI operates on pattern-matching defaults — producing plausible output that is invisible to the governance record.
::

## From Constraint to Practice: How Prompts Operationalize Governance

A session mandate is not automatic — it must be established at the start of every AI session. This is where the AI Playbook's prompt collections become practical. Each prompt is structured to do three things: establish the session context, state the constraints explicitly, and define the expected output in terms the AI can act on.

The Guides section provides per-stratum prompt collections for GitHub Copilot, Cursor, and Claude. Each prompt is ready to paste into a new chat window — no adaptation required for standard STRATA projects. For projects with non-standard structures, the Usage Notes on each prompt specify exactly which values to substitute.
