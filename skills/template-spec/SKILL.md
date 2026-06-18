---
name: template-spec
description: Project specification template for AI coding agents. Covers tech stack, architecture, data models, API endpoints, phased builds, and handoff protocol.
---
# Template — AI Agent Project Spec

A structured template for writing a complete **System Directive & Implementation Guide** to send to an AI coding agent (Claude, GPT, Gemini, etc.) to build a project from scratch.

## When to invoke

- "create a project spec" / "write a spec for my project"
- "help me spec out [project name]"
- "I want to build [X], help me write the full directive for the agent"
- "project template" / "implementation guide" / "system directive"

## When NOT to use

- You just want to discuss an idea — that's architecture-estimation.
- The project is already built and you want to document it — that's teaching-writing.

---

## How it works

The full template lives in [`docs/project-spec-template.md`](../docs/project-spec-template.md).

When invoked, ask the user the **7 Questionnaire questions** (Section 1 of the template), then fill out the template sections that apply based on their answers. Deliver the completed spec as a single markdown document they can paste into a new agent conversation.

## The 7 Questions (always ask these first)

1. **Project name & tagline** — what is it called and what does it do in 5 words?
2. **What does it do?** — 2–4 sentences: what, who, why, where deployed.
3. **Complexity** — Simple (CRUD/landing page) / Medium (business logic, integrations, auth) / Complex (multi-service, real-time, AI)?
4. **Deployment scale** — Local only / LAN / Cloud?
5. **Users** — Just me / Small team (2–10) / Public?
6. **Your coding level** — Beginner / Intermediate / Advanced?
7. **Code comment language** — English / Thai / other?

## Output format

Produce a single markdown document using the template structure:
- Section 2: Context & Role
- Section 3: Operational Rules (Phase Rule, Agent Invariant Rules, Ponytail Rule, Handoff Protocol)
- Section 4: Project Overview
- Section 5: Tech Stack
- Section 6: Architecture (if Medium/Complex)
- Section 7: Directory Structure
- Section 8: Data Models (if applicable)
- Section 9: Config & DB Setup (if applicable)
- Section 10: Auth (if Multi-user or Public)
- Section 11: API Endpoints (if applicable)

Skip sections that don't apply based on questionnaire answers.

## Operating rules

- Ask all 7 questions **first** before writing anything. Don't guess.
- Conditionally include/exclude sections based on Q3 (complexity), Q4 (scale), Q5 (users).
- If the user answers mid-spec and the answers change something already written, update it.
- Ponytail applies: for a Simple/Local/Just-me project, skip auth, skip complex infra, skip the sections that don't exist.
