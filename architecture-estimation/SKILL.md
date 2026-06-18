---
name: architecture-estimation
description: Framework for system design, tradeoff evaluation, and task estimation.
---

# Architecture & Estimation

## System Design

Think before building. Evaluate tradeoffs, question assumptions, and choose the
simplest architecture that meets the real constraints — not the imagined ones.

### When to invoke

- "design a system for…" / "architect this" / "how should I structure…"
- "what's the right pattern for…" / "monolith or microservice?"
- "evaluate these options" / "tradeoff analysis"
- "tech stack decision" / "should we use X or Y?"
- Starting a new project, feature, or service from scratch.

### When NOT to use

- **Reviewing existing code** — that's Scrutinize.
- **Reducing complexity in existing code** — that's Ponytail Review/Audit.
- **The decision is already made and the user wants implementation** — just code.

---

### Workflow — 4 steps

#### 1. Clarify constraints before proposing anything

Ask (don't assume) about the constraints that actually matter:

- **Scale:** How many users / requests / records? Today and in 12 months.
- **Team:** How many engineers will touch this? What do they already know?
- **Timeline:** Ship in a week or a quarter?
- **Existing infra:** What's already running? What can we reuse?
- **Operational cost:** Who maintains this at 3am?

If the user doesn't know, state your assumption explicitly:
*"Assuming <1000 req/s and a 2-person team — this changes if either grows 10x."*

Cap clarification at **3 questions max**. Don't interrogate — gather enough to
make a defensible default, then propose.

#### 2. Propose the simplest viable architecture

Apply the Ponytail ladder to architecture:

1. **Does this component need to exist at all?**
2. **Can the platform / framework / managed service handle it?**
3. **Can an existing part of the system absorb this responsibility?**
4. **Only then: add a new component.**

For each component in the design, state:

- **What it does** (one sentence).
- **Why it exists** (what breaks without it).
- **What it talks to** (inputs/outputs).

Format: a short text description or a simple diagram (Mermaid if the user's
tooling supports it). No 15-box diagrams for a 3-component system.

#### 3. Present tradeoffs honestly

For every non-trivial decision, show a **tradeoff table**:

| Option | Pro | Con | Choose when |
|--------|-----|-----|-------------|
| A | ... | ... | ... |
| B | ... | ... | ... |

Rules:

- **Name the option you'd pick** and say why in one sentence. Don't hide behind
  "it depends" when you have enough context to decide.
- **Name what you're giving up.** Every architecture decision is a bet — make the
  bet visible.
- **Don't present more than 3 options.** Two is usually enough. Five is analysis
  paralysis.

#### 4. Call out the ceilings

Mark known limitations with a `ponytail:` style note:

*"This design handles 1K req/s. At 10K, the single DB becomes a bottleneck —
shard or add a read replica."*

These ceilings become the upgrade triggers — same convention as code-level
`ponytail:` comments.

---

### Operating rules

- **No résumé-driven architecture.** Don't propose Kubernetes for a cron job.
  Don't propose event sourcing for a CRUD app. The Ponytail ladder applies to
  architecture the same way it applies to code.
- **Boring tech wins by default.** Postgres over a new DB. REST over GraphQL
  unless the client needs demand it. Monolith over microservices until the team
  outgrows it. Propose the exciting option only when the boring one measurably
  fails.
- **Reversibility matters.** Prefer decisions that are cheap to reverse. Flag
  irreversible decisions explicitly: *"This is a one-way door — worth extra
  scrutiny."*
- **Don't over-diagram.** A three-sentence description of data flow beats an
  architecture astronaut's 20-box diagram. Diagrams serve communication, not
  decoration.
- **State assumptions, not certainties.** "I'm assuming auth is handled upstream"
  is useful. "Auth is handled upstream" without checking is a bug.

---

## Estimation

Break tasks down, size them, surface unknowns. The goal is not a precise number
— it's a shared understanding of what's big, what's small, and what's uncertain.

### When to invoke

- "how long will this take?" / "estimate this" / "size this"
- "break this down" / "task breakdown" / "what's the effort?"
- "t-shirt size" / "story points" / "sprint planning"
- "is this a small change or a big one?"

### When NOT to use

- **User wants you to just build it.** Build it.
- **Trivial question** ("add a log line") — answer "XS, minutes" and move on.

---

### Workflow

#### 1. Decompose into atomic tasks

Break the work into pieces small enough that each one is:

- **One concern** (not "build the API and write the tests and deploy").
- **Estimable** (you can reason about effort without hand-waving).
- **Independently deliverable** (could be a single PR).

Use a flat list or shallow nesting (max 2 levels). Deep task trees are planning
theater.

#### 2. Size each task

Use T-shirt sizes with consistent meaning:

| Size | Typical effort | Characteristics |
|------|---------------|-----------------|
| **XS** | < 2 hours | Config change, copy fix, one-liner |
| **S** | 2–8 hours | Simple feature, straightforward bug fix, clear path |
| **M** | 1–3 days | Moderate complexity, some unknowns, touches 2–3 files/modules |
| **L** | 1–2 weeks | Cross-cutting concern, significant unknowns, needs design |
| **XL** | > 2 weeks | Major feature, architectural change, high uncertainty |

Rules:

- **When between sizes, round up.** Optimistic estimates are the #1 cause of
  blown timelines.
- **State your assumptions** for each size. "S assuming the API already exists"
  is more useful than just "S".
- **Flag unknowns explicitly.** If you can't size something, say why:
  *"Can't estimate token refresh — need to read the OAuth provider's docs first.
  Estimate: S–L depending on what we find."*

#### 3. Output format

```
## [Feature Name] — Total: [aggregate size]

1. [Task description] — [Size]
   Assumption: [why this size]
2. [Task description] — [Size]
   ⚠️ Unknown: [what needs investigation]
3. ...

### Unknowns that block estimation
- [What we don't know yet] → [what to do to find out]
```

#### 4. Guard against false precision

- **Never give hour estimates unless explicitly asked.** T-shirt sizes
  communicate uncertainty honestly. "3.5 days" implies a precision you don't
  have.
- **Don't estimate what you don't understand.** If a task is unclear, the
  estimate is "needs spike" — not a guess.
- **Re-estimate after unknowns are resolved.** The first estimate is a draft,
  not a contract.

---

### Operating rules

- **Decompose first, estimate second.** Never estimate a blob. Break it down,
  then size the pieces.
- **Conservative by default.** The Ponytail philosophy applies: the laziest
  honest estimate is the one that doesn't lie to the team about what's involved.
- **Separate known from unknown.** A clear list of unknowns is more valuable
  than a confident estimate that hides them.
- **Don't manufacture precision.** If the user asks "how many hours?", give a
  range and explain the uncertainty. *"8–24 hours depending on whether the
  third-party API has pagination"* is honest. *"16 hours"* is a fiction.
