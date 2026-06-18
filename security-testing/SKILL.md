---
name: security-testing
description: Security vulnerability checklist and testing strategy guidelines.
---
# Security & Testing

## Security Review

Systematic check for security vulnerabilities. Ponytail Review explicitly
excludes security — this skill picks up what it leaves behind.

### When to invoke

- "security review" / "security audit" / "check for vulnerabilities"
- "is this safe?" / "any security issues?"
- "OWASP check" / "pen test this" / "threat model"
- Before deploying a new feature that handles auth, payments, user data, or
  external input.

### When NOT to use

- **General code review** — that's Scrutinize.
- **Over-engineering review** — that's Ponytail Review.
- **Incident in progress** — that's Incident Response. Secure later, restore now.

---

### Workflow

#### 1. Identify trust boundaries

Before looking at any code, map where untrusted input enters the system:

- User input (forms, URL params, headers, file uploads)
- API requests from external services
- Data from third-party dependencies
- Database content that was originally user-supplied
- Environment variables and config files

Every trust boundary is an attack surface. Focus the review here.

#### 2. Run the checklist

Check each category. For each finding: state the vulnerability, cite the
location, and give a concrete fix.

##### Input & Output

- [ ] **Injection (SQL, NoSQL, command, LDAP).** Is user input ever interpolated
  into queries or commands without parameterization?
- [ ] **XSS (Cross-Site Scripting).** Is user-supplied content rendered in HTML
  without escaping? Check both stored and reflected XSS.
- [ ] **Path traversal.** Can user input influence file paths? Is it sanitized
  against `../`?
- [ ] **Deserialization.** Is untrusted data deserialized without validation?
  (pickle, eval, JSON.parse of executable content)
- [ ] **Output encoding.** Is output escaped for the correct context (HTML, URL,
  JS, CSS, SQL)?

##### Authentication & Authorization

- [ ] **Auth bypass.** Can any endpoint be reached without proper authentication?
- [ ] **Broken access control.** Can user A access user B's data by changing an
  ID in the URL/body? (IDOR)
- [ ] **Privilege escalation.** Can a normal user reach admin functionality?
- [ ] **Session management.** Are tokens/cookies secure, HttpOnly, SameSite?
  Proper expiry? Revocation on logout?
- [ ] **Password handling.** Hashed with bcrypt/argon2? Never logged? Never
  returned in API responses?

##### Data Protection

- [ ] **Secrets in code.** API keys, passwords, tokens hardcoded or committed?
  Check `.env` files, config, git history.
- [ ] **Sensitive data exposure.** PII in logs? Sensitive fields in error
  messages? Overly verbose API responses?
- [ ] **Encryption.** TLS for all external communication? Sensitive data
  encrypted at rest?
- [ ] **CORS.** Is the CORS policy restrictive enough? Wildcard `*` on
  authenticated endpoints is a vulnerability.

##### Dependencies & Infrastructure

- [ ] **Known CVEs.** Are dependencies up to date? Run `npm audit` /
  `pip-audit` / `cargo audit` / equivalent.
- [ ] **Supply chain.** Any dependencies with < 100 stars, single maintainer, or
  recent ownership transfers?
- [ ] **Dockerfile / infra.** Running as root? Unnecessary ports exposed? Debug
  mode in production?

##### Logic & Rate Limiting

- [ ] **Rate limiting.** Are auth endpoints (login, register, password reset)
  rate-limited?
- [ ] **Race conditions.** Can concurrent requests cause double-spending, double
  voting, or TOCTOU bugs?
- [ ] **Business logic abuse.** Can the happy path be abused? (e.g., negative
  quantities, free trial loops, referral exploits)

#### 3. Report

One section per finding, ordered by severity:

- **Finding** — one sentence, specific. Cite `file:line`.
- **Severity** — Critical / High / Medium / Low.
- **Impact** — what an attacker can do.
- **Fix** — concrete, minimal. Code snippet or configuration change.

End with a summary: `<N> findings: <critical> critical, <high> high, <medium>
medium, <low> low.`

If nothing found: *"No vulnerabilities found in the reviewed surface. Checked:
[list what you checked]. Not checked: [list what you didn't]."* Never claim
clean without stating the scope.

---

### Operating rules

- **No false sense of security.** State what you checked and what you didn't.
  "Reviewed auth flow and input handling; did not review infrastructure or
  third-party integrations" is honest.
- **Don't pad with noise.** If you find one Critical, lead with it. Don't bury
  it under 10 Low-severity nits.
- **Concrete fixes only.** "Improve input validation" is not actionable. "Use
  parameterized queries in `db.query()` at `users.py:42`" is.
- **This is not a pentest.** You're reviewing code, not exploiting a running
  system. Flag what looks dangerous; the user decides whether to test it.
- **Tools are complements, not replacements.** Suggest running `npm audit`,
  `semgrep`, `bandit`, or similar when appropriate — but don't skip the
  manual checklist.

---

## Testing Strategy

Plan what to test, how to test it, and where the gaps are. This skill activates
when the user wants more than the Ponytail minimum (one assert-based self-check)
and needs a real test plan.

### When to invoke

- "write a test plan" / "testing strategy" / "what should I test?"
- "test coverage" / "where are the gaps?" / "what's not tested?"
- "should this be a unit test or integration test?"
- "set up testing for this project"
- Any time the user asks for comprehensive testing beyond a single self-check.

### When NOT to use

- **Simple one-liner or trivial logic.** Ponytail says no test needed. Don't
  manufacture ceremony.
- **Debugging a specific test failure.** That's Debug Mantra.
- **Reviewing test quality.** That's Scrutinize, applied to test code.

---

### Workflow

#### 1. Map the testing surface

Before writing any tests, identify:

- **Critical paths** — what must never break? (auth, payments, data integrity)
- **Complex logic** — where are the branches, loops, parsers, state machines?
- **Integration points** — what talks to external systems? (DB, API, file system)
- **User journeys** — what does the user actually do end-to-end?

#### 2. Assign test levels

For each area, choose the right level. Use the **testing pyramid** — more at the
bottom, fewer at the top:

| Level | What it tests | Speed | When to use |
|-------|--------------|-------|-------------|
| **Unit** | Single function/class in isolation | Fast | Pure logic, calculations, transformations, validators |
| **Integration** | Component interactions, DB queries, API calls | Medium | Anything that crosses a boundary (DB, network, file system) |
| **E2E** | Full user journey through the real system | Slow | Critical happy paths, smoke tests, deployment validation |

Rules:

- **Unit test first by default.** If the logic can be tested in isolation,
  isolate it. Unit tests are fast, reliable, and cheap.
- **Integration test when boundaries matter.** A mocked DB test that passes but
  a real DB test that fails is a false positive. Test the boundary.
- **E2E sparingly.** E2E tests are expensive, flaky, and slow. Use them for the
  3–5 most critical user journeys, not for exhaustive coverage.
- **Don't mock what you own.** Mocking your own code hides bugs. Mock external
  dependencies you can't control.

#### 3. Identify what NOT to test

Ponytail applies to tests too:

- Don't test framework behavior (does React render a div? Yes.)
- Don't test trivial getters/setters.
- Don't test private implementation details — test the public interface.
- Don't write tests that pass regardless of correctness (assertions on
  intermediate state, empty test bodies, tests that catch all exceptions).

#### 4. Output format

```
## Test Plan — [Feature/Module Name]

### Unit Tests
- [ ] [What to test] — [Why it matters]
  Input: [example] → Expected: [result]
- [ ] ...

### Integration Tests
- [ ] [What to test] — [What boundary it exercises]
- [ ] ...

### E2E Tests (critical paths only)
- [ ] [User journey description]
- [ ] ...

### Not Testing (intentional)
- [What] — [Why it's safe to skip]

### Coverage Gaps (known risks)
- [What's not covered] — [Why / what would it take to cover]
```

---

### Operating rules

- **Coverage is a signal, not a goal.** 90% coverage with bad assertions is
  worse than 60% coverage of the right things. Optimize for catching real bugs,
  not hitting a number.
- **Test behavior, not implementation.** If you refactor the internals and all
  tests break, those tests were testing the wrong thing.
- **Name the tradeoff.** "Skipping integration tests for the cache layer —
  risk: cache invalidation bugs. Add when: cache logic becomes non-trivial."
- **One concern per test.** A test that checks auth AND input validation AND
  response format is three tests pretending to be one.
- **Flaky tests are worse than no tests.** A test that fails randomly trains the
  team to ignore failures. Fix it or delete it.
