---
name: refactoring-performance
description: Safe refactoring guidelines and performance profiling strategies.
---

# Refactoring & Performance

## Refactoring Guide

Change structure without changing behavior — safely. Ponytail Audit tells you
*what* to cut; this skill tells you *how* to cut it without breaking things.

### When to invoke

- "refactor this" / "restructure" / "reorganize"
- "extract this into…" / "inline this" / "simplify this module"
- "migrate from X to Y" / "strangler fig" / "incremental rewrite"
- "how do I safely change this without breaking things?"
- After a Ponytail Audit identifies things to cut.

### When NOT to use

- **Adding new features.** Just build them.
- **Reviewing code for over-engineering.** That's Ponytail Review.
- **Bug fix that happens to clean things up.** Fix the bug first (Debug Mantra),
  clean up second.

---

### Workflow — 4 steps

#### 1. Establish the safety net

**Before touching anything:**

- **Do tests exist?** If yes, run them. They must pass before any refactoring
  starts. This is your rollback signal.
- **No tests?** Write the minimum tests that cover the *current behavior* of the
  code you're about to change. Test the public interface, not the internals.
  These tests are your insurance — they prove the refactor didn't change
  behavior.
- **Can't write tests?** (e.g., tightly coupled legacy code) Then use the
  smallest possible refactoring steps, verify manually after each one, and
  commit at each step so you can revert.

**The rule:** every refactoring step must be verifiable. If you can't verify it,
the step is too big — break it down.

#### 2. Plan the sequence

Refactoring is a sequence of small, safe transformations — not a rewrite.

Plan the steps before executing:

```
1. [What to change] → verify: [how to confirm it didn't break]
2. [What to change] → verify: [how to confirm]
3. ...
```

Common safe transformations (pick the ones that apply):

| Transformation | When to use | Risk |
|---------------|-------------|------|
| **Extract function/method** | Long function with distinct sections | Low — behavior preserved if tests pass |
| **Inline function** | Wrapper that only delegates | Low — removing indirection |
| **Rename** | Name doesn't match what it does | Low — but update all call sites |
| **Move to new file/module** | File doing too many things | Low — update imports |
| **Replace conditional with polymorphism** | Switch/if chain growing | Medium — changing structure |
| **Extract interface** | Multiple implementations emerging | Medium — introducing abstraction |
| **Strangler fig** | Replacing a large module incrementally | High — needs parallel running |

#### 3. Execute — one step at a time

For each step in the sequence:

1. **Make the change.** One transformation only.
2. **Run the tests.** All tests must pass.
3. **Commit.** Each step is its own commit. If the next step goes wrong, you
   revert one commit — not the whole refactor.

Rules:

- **Never combine refactoring with behavior changes.** If you need to add a
  feature AND refactor, do them in separate PRs. Mixing them makes review
  impossible and rollback dangerous.
- **Never refactor code you don't understand.** Read first, understand the
  behavior, then change the structure. Refactoring without understanding is
  how you introduce bugs that tests can't catch (because the tests are also
  wrong).
- **If tests break, the refactor is wrong.** Don't fix the tests to match your
  refactoring — that's changing behavior, not structure. Revert the step and
  try a different approach.

#### 4. Verify the result

After all steps are complete:

- Run the full test suite.
- Check that the public API / interface hasn't changed (unless that was the
  explicit goal).
- Confirm the code is actually simpler. If the refactored version has more lines,
  more files, or more abstractions than the original — question whether the
  refactoring was worth it.

---

### Migration patterns

For larger-scale refactoring (replacing a module, migrating to a new framework):

#### Strangler Fig

Replace a legacy component incrementally without a big-bang rewrite:

1. **Identify the boundary.** Where does the old component interface with the
   rest of the system?
2. **Build the new component** behind the same interface.
3. **Route traffic incrementally.** Feature flag, proxy, or adapter that sends
   some requests to the new component.
4. **Verify parity.** Both old and new produce the same results for the same
   inputs.
5. **Increase traffic** to the new component until 100%.
6. **Remove the old component.**

#### Parallel Run

When you can't afford to get migration wrong (financial, data integrity):

1. Run both old and new implementations simultaneously.
2. Compare outputs. Log discrepancies.
3. Fix the new implementation until discrepancies reach zero.
4. Cut over.

---

### Operating rules

- **Tests before refactoring. Always.** No safety net = no refactoring. Write
  the tests first or accept the risk explicitly.
- **Small steps, frequent commits.** The ideal refactoring step is so small it's
  boring.
- **Don't refactor everything at once.** Ponytail Audit might list 20 findings.
  Pick the highest-impact 3, refactor those, ship, then reassess. Avoid the
  "while I'm in here" trap.
- **Separate refactoring PRs from feature PRs.** Always. Non-negotiable for
  reviewability.
- **The Ponytail question applies:** "Does this refactoring need to happen at
  all?" If the code works, is readable, and isn't blocking anything — leave it.
  Refactoring for aesthetics is overhead.

---

## Performance

Measure first, optimize second. The goal is to find and fix the actual
bottleneck — not the one you imagine.

### When to invoke

- "it's slow" / "performance issue" / "optimize this"
- "profile this" / "benchmark" / "find the bottleneck"
- "why is this taking so long?" / "latency is too high"
- "memory usage" / "CPU usage" / "this is eating resources"

### When NOT to use

- **Production outage.** That's Incident Response — restore first, optimize later.
- **Premature optimization.** If nobody complained and no metric is red, don't
  optimize. Build first, measure later.
- **Micro-optimizing code that isn't in the hot path.** Saving 2ms in a function
  called once per day is not worth the complexity.

---

### The Performance Mantra

> 1. **Measure before changing anything.**
> 2. **Find the bottleneck.** Don't guess — profile.
> 3. **Change one thing.** Measure again.
> 4. **Stop when the target is met.**

### Workflow

#### 1. Define the target

Before optimizing, establish:

- **What metric matters?** Latency (p50/p95/p99)? Throughput (req/s)? Memory?
  CPU? Startup time?
- **What's the current value?** Measure it. This is your baseline.
- **What's the target value?** "Faster" is not a target. "p95 < 200ms" is.
- **Where is this measured?** Local dev machine? Staging? Production? The
  environment affects the numbers.

If the user says "it's slow" without a target, ask: *"What's acceptable?
What's it at now?"*

#### 2. Profile — find the actual bottleneck

**Do not guess.** Common tools by environment:

| Environment | Tools |
|-------------|-------|
| **Python** | `cProfile`, `py-spy`, `line_profiler`, `memory_profiler` |
| **JavaScript/Node** | Chrome DevTools, `clinic.js`, `0x`, `--inspect` |
| **Go** | `pprof`, `trace`, benchmark tests |
| **Rust** | `perf`, `flamegraph`, `criterion` |
| **JVM** | `async-profiler`, `VisualVM`, `JFR` |
| **Database** | `EXPLAIN ANALYZE`, slow query log, `pg_stat_statements` |
| **Web (frontend)** | Lighthouse, Chrome Performance tab, Web Vitals |
| **General** | `time`, `htop`, `strace`/`dtrace`, flame graphs |

Focus on:

- **Where is time spent?** The top 3 functions/queries by cumulative time are
  your candidates.
- **What's the call count?** A fast function called 10,000 times is worse than
  a slow function called once.
- **I/O vs. CPU.** Is the bottleneck computation or waiting? (network, disk, DB)
  The fix is different for each.

#### 3. Optimize the bottleneck — and only the bottleneck

Common fixes by bottleneck type:

| Bottleneck | Typical fix |
|-----------|------------|
| **N+1 queries** | Batch/join instead of loop |
| **Missing index** | Add the index (check write impact) |
| **Unnecessary computation** | Cache the result (`lru_cache`, memoize, Redis) |
| **Synchronous I/O** | Make it async, or parallelize |
| **Large payload** | Paginate, compress, or return less data |
| **Algorithmic** | Better data structure (set instead of list, hash map instead of linear scan) |
| **Memory** | Stream instead of load-all, reduce copies, pool allocations |
| **Frontend render** | Virtualize long lists, lazy-load, debounce, avoid layout thrash |

Rules:

- **One change per measurement cycle.** Change two things at once and you don't
  know which one helped (or hurt).
- **Measure after each change.** Compare against the baseline, not your memory
  of the baseline. Use numbers.
- **Ponytail applies.** The laziest fix that meets the target is the right fix.
  Don't rewrite the module when adding an index solves it.

#### 4. Stop when the target is met

If the metric hits the target: **stop optimizing.** Further optimization is
premature optimization by definition — you've already met the goal.

Document what you did and what the numbers look like now:

```
Baseline:  p95 = 850ms, avg = 420ms
After fix: p95 = 180ms, avg = 95ms (added index on orders.user_id)
Target:    p95 < 200ms ✅
```

If the target is NOT met after optimizing the top bottleneck, re-profile. The
bottleneck has shifted — don't keep optimizing the same thing.

---

### Operating rules

- **Measure first. Always.** "I think this is slow because…" is not evidence.
  A profile is evidence.
- **No premature optimization.** Ponytail says: don't solve problems that don't
  exist yet. Build it simple, measure it, optimize only what's measured as slow.
- **Optimize the bottleneck, not the code you're comfortable with.** The
  bottleneck is rarely where you expect it.
- **Numbers, not feelings.** "It feels faster" is not validation. Before/after
  numbers in the same environment with the same data is validation.
- **Document the win.** A `ponytail:` comment at the optimization site:
  `// ponytail: indexed user_id for p95 < 200ms; revisit if write volume 10x`
- **Performance regressions are bugs.** If an optimization regresses something
  else, revert it. Optimization that trades one problem for another is not
  optimization.
