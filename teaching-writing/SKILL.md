---
name: teaching-writing
description: Socratic mode for teaching and templates for technical documentation.
---

# Teaching & Writing

## Socratic Mode

Guide through questions instead of answers. The user learns by thinking, not by
copying. Switch from "give the solution" to "ask the question that makes the
solution obvious."

### When to invoke

- "teach me" / "explain why" / "help me understand"
- "pair with me" / "socratic mode" / "don't give me the answer"
- "walk me through this" / "mentor mode"
- When the user is explicitly learning a new concept, language, or pattern.

### When NOT to use

- **User wants a quick answer.** "What's the method to sort an array?" → just
  answer. Socratic mode on syntax lookups is obnoxious.
- **Production incident or time pressure.** Don't teach during a fire.
- **User says "just tell me" or "stop asking."** Immediately switch back to
  normal mode. Respect the off-ramp.

---

### How it works

#### The core rule

**Ask, don't tell.** Before giving any information:

1. Ask what the user already knows or has tried.
2. Ask a question that guides them toward the next insight.
3. Wait for their response before proceeding.

#### Question types (use in this order of preference)

1. **Diagnostic** — surface what they already know.
   *"What do you think happens when this function is called with an empty list?"*

2. **Narrowing** — reduce the problem space.
   *"Which of these three variables changes between the working case and the
   broken case?"*

3. **Connecting** — link to something they already understand.
   *"This is similar to the iterator pattern you used last week — how did you
   handle the end-of-sequence case there?"*

4. **Challenging** — test the edges of their understanding.
   *"What happens if two users call this endpoint at the same time?"*

5. **Hint** — when they're truly stuck, offer a nudge, not the answer.
   *"Look at what type `response.data` actually is. Is it what you expected?"*

#### One question at a time

Ask ONE question, then stop. Wait for the answer. Don't ask five questions in a
row — that's an exam, not teaching.

#### When to break Socratic mode temporarily

- The user is stuck after 3 exchanges on the same point → give a **partial**
  answer ("The key insight is X — now, given that, what would you do?").
- The user asks a factual/syntax question → answer it directly, then return to
  Socratic.
- The user is frustrated → acknowledge it, offer to switch: *"Want me to just
  show you the solution, or keep working through it?"*

#### Semi-Socratic mode

If the user wants guidance but not full Socratic rigor:

- Explain the concept briefly.
- Then ask one follow-up question to check understanding.
- Proceed based on their answer.

Trigger: "semi-socratic" or "explain but quiz me."

---

### Operating rules

- **One question per turn.** Not two. Not a question with a follow-up. One.
- **Don't pretend not to know.** If the user asks "do you know the answer?" —
  yes, you do. The point is for them to find it, not for you to roleplay
  ignorance.
- **Praise sparingly, specifically.** "Good" is filler. "Right — and that's
  exactly why the closure captures the variable by reference, not by value" is
  teaching.
- **Track where they are.** If they answered the last question correctly, move
  forward. If they didn't, don't skip ahead — address the gap.
- **Exit cleanly.** When the user reaches understanding, summarize what they
  learned in 2–3 sentences. Don't drag it out.
- **Respect "just tell me."** The moment the user wants out, switch to normal
  mode. No pushback, no "are you sure?"

---

## Technical Writing

Structured documentation with consistent quality. Each doc type has a template
and a standard — no more random-quality READMEs and half-finished runbooks.

### When to invoke

- "write a README" / "write documentation" / "document this"
- "write an ADR" / "architecture decision record"
- "write a runbook" / "playbook" / "ops doc"
- "write a changelog" / "release notes"
- "API documentation" / "write the docs"

### When NOT to use

- **Writing for non-engineers.** That's Management Talk.
- **Writing a post-mortem.** That's the Post-mortem skill.
- **Inline code comments.** Just write them — no skill needed.

---

### Doc types and templates

#### README

Audience: a new engineer opening the repo for the first time.

```markdown
# [Project Name]

[One paragraph: what this does and why it exists.]

## Quick Start

[Minimum steps to get it running locally — aim for copy-paste-done.]

### Prerequisites
- [Runtime/tool] [version]

### Install
[commands]

### Run
[commands]

### Verify
[How to know it's working — a curl, a test, a URL to visit.]

## Architecture

[2–5 sentence overview of the major components and how they connect.
Diagram only if genuinely useful.]

## Development

### Project Structure
[Key directories and what they contain. Not every file — just the ones
a newcomer needs to find.]

### Testing
[How to run tests. What kind of tests exist.]

### Deployment
[How this gets to production. Link to CI/CD config if relevant.]

## Contributing

[Branch naming, PR process, review expectations. Keep it short.]
```

Rules:
- **Quick Start must work.** If a new engineer follows it and it fails, the
  README is broken.
- **No aspirational content.** Don't document features that don't exist yet.
- **Link, don't duplicate.** If detailed API docs live elsewhere, link to them.

---

#### ADR (Architecture Decision Record)

Audience: future-you and your team, 6 months from now, asking "why did we do
it this way?"

```markdown
# ADR-[NNN]: [Decision Title]

**Status:** [Proposed | Accepted | Deprecated | Superseded by ADR-XXX]
**Date:** [YYYY-MM-DD]
**Deciders:** [Names]

## Context

[What is the problem or situation that requires a decision?
What constraints apply? What forces are in tension?]

## Decision

[What did we decide? State it directly.]

## Alternatives Considered

### [Alternative A]
- Pro: ...
- Con: ...
- Rejected because: ...

### [Alternative B]
- Pro: ...
- Con: ...
- Rejected because: ...

## Consequences

### Positive
- [What gets better]

### Negative
- [What gets worse or becomes harder]

### Risks
- [What could go wrong with this decision]
```

Rules:
- **Context is king.** The decision itself is often obvious in hindsight — the
  context that led to it is what gets lost. Over-invest in context.
- **List alternatives you actually considered**, not strawmen. If you only
  considered one option, say so — that's a signal worth recording.
- **Negative consequences are mandatory.** Every decision has tradeoffs. If you
  can't name a downside, you haven't thought hard enough.

---

#### Runbook / Playbook

Audience: the on-call engineer at 3am who has never seen this system before.

```markdown
# Runbook: [Scenario / Alert Name]

## Trigger
[What alert, symptom, or situation activates this runbook?]

## Impact
[What's affected if this isn't resolved? Severity level.]

## Diagnosis

### Step 1: [Check]
```command
[exact command to run]
```
Expected output: [what "healthy" looks like]
If unhealthy: [go to step N / escalate]

### Step 2: [Check]
...

## Resolution

### Option A: [Fix Name]
```command
[exact commands]
```
Verify: [how to confirm it worked]

### Option B: [Fallback]
...

## Rollback
[If the fix makes things worse, how to undo it.]

## Escalation
[Who to contact if this runbook doesn't resolve the issue. Names, channels.]

## History
[Past incidents that used this runbook and any lessons learned.]
```

Rules:
- **Commands must be copy-pasteable.** No "run the appropriate command" — write
  the actual command.
- **Every step has a verification.** "Did it work?" must have a concrete answer.
- **Include rollback.** Every fix can make things worse. How to undo it.
- **Keep it updated.** A runbook that references last year's infrastructure is
  dangerous. Date-stamp it.

---

#### Changelog / Release Notes

Audience: developers consuming your library/API, or internal teams tracking
what shipped.

```markdown
# Changelog

## [Version] — [YYYY-MM-DD]

### Added
- [New feature or capability]

### Changed
- [Behavior changes, API changes]

### Fixed
- [Bug fixes]

### Removed
- [Deprecated features removed]

### Breaking
- [Changes that require action from consumers]
  Migration: [how to update]
```

Rules:
- **User-facing, not developer-facing.** "Fixed null pointer in
  `parseResponse`" → "Fixed crash when API returns empty response."
- **Breaking changes get migration instructions.** Always.
- **Group by impact, not by commit order.**

---

### Operating rules

- **Match the template.** Use the right template for the doc type. Don't write a
  README-shaped ADR.
- **One pass, then offer revision.** Produce the full draft, then ask if
  anything needs adjustment. Don't ask 10 questions before writing.
- **Don't invent facts.** If you don't know the deploy process, leave a
  `[TODO: deployment process]` placeholder. Don't guess.
- **Ponytail still applies.** The shortest README that gets a new engineer
  productive is the right README. No 50-page opus for a 200-line script.
