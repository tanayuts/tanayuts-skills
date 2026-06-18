---
name: incident-response
description: Real-time 5-step protocol for handling production incidents and outages.
---
# Incident Response

Real-time discipline for production incidents. **Not** a post-mortem (that's
after the fire is out). This is the fire drill: triage, stop the bleeding,
communicate, then hand off to the post-mortem pipeline.

## When to invoke

- "prod is down" / "production incident" / "outage" / "site is down"
- "customers can't [X]" / "alerts firing" / "pager went off"
- "we need to roll back" / "something broke in prod"
- "incident" / "sev 1" / "sev 2" / "P0" / "P1"

If severity is unclear, ask one question: *"Is this customer-visible right
now?"* and classify from the answer.

## When NOT to use

- **Bug not in production yet.** That's Debug Mantra territory.
- **Incident already resolved, writing it up.** That's Post-mortem.
- **Performance degradation you're investigating at leisure.** That's normal
  debugging or the Performance skill.

---

## The Protocol — 5 steps, in order

### 1. Triage — what is actually happening?

Answer these in under 60 seconds of reading:

- **What's broken?** One sentence. Service name, endpoint, feature.
- **Who's affected?** Customers / internal / subset / everyone.
- **Since when?** First alert timestamp, or user's best guess.
- **Severity?** Use the team's scale, or default to:

| Severity | Meaning |
|----------|---------|
| **SEV-1** | Customer-visible, revenue-impacting, no workaround |
| **SEV-2** | Customer-visible, workaround exists or subset affected |
| **SEV-3** | Internal-only or non-critical degradation |

- **What changed recently?** Last deploy, config change, infra event, dependency
  update. The most recent change is the prime suspect until proven otherwise.

Output a **one-paragraph situation summary** the moment triage completes. This
becomes the first entry in the timeline (step 4).

### 2. Mitigate — stop the bleeding before diagnosing

The goal is **restore service**, not find root cause. Diagnosis comes later.

Pick the fastest safe option:

1. **Rollback** the last deploy / config change.
2. **Feature-flag off** the suspected feature.
3. **Redirect traffic** away from the broken path (failover, circuit breaker).
4. **Scale up / restart** if the failure is resource exhaustion.
5. **Hotfix** only if the above won't work and the fix is obvious and small.

Rules:

- **Mitigate first, debug second.** Do not spend 30 minutes chasing root cause
  while customers are down. A rollback that restores service in 2 minutes beats
  a perfect diagnosis that takes 45.
- **One change at a time.** If the first mitigation doesn't work, revert it
  before trying the next. Stacking mitigations creates a new incident.
- **Announce the mitigation** before executing it (step 3).
- **Verify the mitigation worked.** Check the same signal that fired the alert.
  If it's still firing, the mitigation failed — try the next option.

### 3. Communicate — keep stakeholders informed

Draft comms **at each phase transition**, not just at the end. Three audiences,
three shapes:

#### Internal engineering (Slack / war-room)

Short, factual, no greeting:

- **What's happening** (one line).
- **What we're doing right now** (one line).
- **Who's on it** (names).
- **ETA for next update** (even if it's "15 min").

Example:
> **[SEV-1] Payment API returning 500s since 14:32 UTC.**
> Rolling back deploy v2.4.7 → v2.4.6. Alex executing, Bo monitoring dashboards.
> Next update in 10 min.

#### Stakeholders (PM, management, support)

Use Management Talk tone from `gem-communication-selfregulation.md`:

- **Status line** (one bold sentence).
- **Customer impact** (what they see).
- **Current action** (what we're doing).
- **ETA** (honest — "unknown" is fine).

No code identifiers. No blame. No speculation about root cause.

#### External (status page / customer-facing)

- **Acknowledge** the issue.
- **State the impact** in user terms ("some users may experience…").
- **Do NOT speculate** on cause or timeline.
- **Update when state changes** (investigating → identified → mitigating →
  resolved).

Rules:

- **Never post to external channels from this skill.** Draft only — the user
  posts it.
- **Update cadence:** every 15–30 min during active incident, even if the update
  is "still investigating, no change."
- **Don't go silent.** Silence during an incident is worse than "no update yet."

### 4. Timeline — log everything as it happens

Maintain a running event log. Each entry:

```
[HH:MM UTC] <who> <what happened> <result>
```

Examples:
```
[14:32] Alert: payment-api error rate >5% (PagerDuty #12345)
[14:35] Alex: confirmed 500s on POST /v1/charges. ~40% of requests failing.
[14:38] Alex: initiating rollback v2.4.7 → v2.4.6
[14:41] Alex: rollback complete. error rate dropping.
[14:45] Bo: error rate back to baseline (<0.1%). monitoring for 15 min.
[15:00] Bo: stable. declaring incident resolved.
```

Rules:

- **Timestamps are mandatory.** UTC preferred.
- **Log mitigations tried AND their results** — including ones that didn't work.
- **This timeline is the raw material for the post-mortem.** Accuracy now saves
  hours of reconstruction later.

### 5. Resolve & Hand off

When the mitigation holds and service is stable:

1. **Declare resolved** — update all three communication channels.
2. **State what fixed it** — "rolled back to v2.4.6" (not root cause, just the
   action that restored service).
3. **Hand off to Post-mortem** — offer to draft using the Post-mortem skill from
   `gem-debugging-review.md`. Pass the timeline as raw material.
4. **List immediate follow-ups** — anything that needs to happen before the next
   business day (revert the revert, temporary monitoring, customer notification).

---

## Operating rules

- **Apply the protocol in order.** Do not skip to step 2 without completing
  step 1. Do not skip to root-cause analysis without attempting mitigation.
- **Bias toward action.** During an incident, a 70% confident rollback is better
  than a 95% confident diagnosis that takes 30 minutes.
- **No blame during the incident.** "The deploy broke it" is fine. "X broke it"
  is not. Blame goes nowhere useful during a fire.
- **Stay in incident mode until service is restored.** Do not pivot to debugging
  mid-incident unless mitigation options are exhausted.
- **The Debug Mantra is for after.** Once service is restored, if root cause is
  still unknown, switch to Debug Mantra for the investigation phase. Then
  Post-mortem for the write-up.
- **Retry cap from Staying on Track applies.** If a mitigation doesn't work
  after 3 attempts, escalate — don't grind.

## Pipeline

This skill connects to the others:

```
[Incident Response] → service restored
        ↓
[Debug Mantra] → root cause found (if not already known)
        ↓
[Post-mortem] → engineering record written
        ↓
[Management Talk] → leadership version (optional)
```
