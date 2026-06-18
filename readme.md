# tanayuts

> Lazy but highly efficient Senior Engineer — YAGNI-focused, systematic debugging, strict PR scrutiny, and full-stack modular toolkit.

This is a **Claude Code plugin** that turns Claude into a senior engineer who writes the minimum code that works and nothing more.

---

## 🚀 How to Install

You can install this plugin directly in **Claude Code** by running:
```bash
claude plugin install github:tanayuts/tanayuts
```

---

## 📁 Repository Structure

```
tanayuts/
├── .claude-plugin/
│   └── plugin.json               # Manifest with modular skills configuration
├── CLAUDE.md                     # Root entry point & Agent operating rules
├── readme.md                     # This file
├── docs/
│   ├── project-spec-template.md  # Standard AI project spec template
│   └── USAGE.md                  # Comprehensive command & usage reference
└── skills/
    ├── coding-philosophy/        # Ponytail YAGNI philosophy (always active)
    ├── debugging-review/         # Debug mantra, PR scrutiny, Post-mortem (RCA)
    ├── incident-response/        # Real-time incident & outage management
    ├── architecture-estimation/  # Tradeoff-driven design & T-shirt estimation
    ├── refactoring-performance/  # Safe refactoring & bottleneck profiling
    ├── security-testing/         # Security vulnerability checklist & testing strategy
    ├── teaching-writing/         # Socratic learning & technical writing templates
    ├── template-spec/            # Project specification generator
    └── communication-selfregulation/ # Executive reporting & stakeholder updates
```

---

## 📖 Command & Trigger Reference

For comprehensive details and quick-reference cards, check out:
* **[Detailed Usage Guide (docs/USAGE.md)](docs/USAGE.md)** — In-depth guide with examples and workflow configurations.
* **[Bilingual Cheat Sheet (docs/CHEATSHEET.md)](docs/CHEATSHEET.md)** — Quick-reference card in both English & Thai (คู่มือสรุปคำสั่งสองภาษา).

Here is a quick summary of the core triggers:

| Trigger / Command | Activates... | Description |
|---|---|---|
| *(any coding task)* | **Ponytail Full Mode** | Enforces the YAGNI ladder & standard library first. |
| `/ponytail lite\|full\|ultra` | **Ponytail Intensity** | Adjusts the ruthlessness of YAGNI pruning. |
| `ponytail-review` | **Ponytail Review** | Scans PR diffs for over-engineering and code bloat. |
| `ponytail-audit` | **Ponytail Audit** | Scans the entire repository for dead code. |
| `ponytail-debt` | **Debt Ledger** | Collects all `// ponytail:` comments into a ledger. |
| `debug` / `error` | **Debug Mantra** | Verbatim 4-step debugging check for reproducible fixes. |
| `scrutinize` | **Scrutinize Review** | Performs an end-to-end code path trace and verification. |
| `/post-mortem` | **Post-mortem (RCA)** | Generates structured engineering root-cause records. |
| `prod is down` / `incident` | **Incident Response** | Triage, rollback, mitigation, and outage updates. |
| `design` / `architect` | **System Design** | Proposes simple architectures with tradeoff tables. |
| `estimate` / `t-shirt size` | **Estimation** | Generates atomic tasks with T-shirt size estimation. |
| `refactor` / `optimize` | **Refactor & Perf** | Safe refactoring steps and performance profiling. |
| `security review` | **Security Review** | OWASP vulnerability checklist and trust boundaries. |
| `test plan` | **Testing Strategy** | Testing pyramid allocation (Unit, Integration, E2E). |
| `teach me` / `socratic` | **Socratic Mode** | Active teaching through guided questioning. |
| `create a project spec` | **Template Spec** | Generates comprehensive specifications for AI agents. |
| `write for management` | **Management Talk** | Re-frames technical updates for Slack/Email/JIRA. |

---

## 💡 Philosophy

**Lazy = efficient, not careless.**

The best code is the code never written. Every line added is a line someone has to read, test, debug, and maintain. This skill set enforces that discipline across the full engineering lifecycle — from architecture down to the PR review.

What it never skips: input validation at trust boundaries, error handling that prevents data loss, security, accessibility, anything you explicitly ask for.

---

## 👥 Credits

This skill set stands on the shoulders of three people:

* **[Ponytail (DietrichGebert)](https://github.com/DietrichGebert/ponytail)** — The core of every file here. The YAGNI ladder, the `ponytail:` comment convention, and the lite/full/ultra intensity levels run straight through coding-philosophy and get applied to architecture, refactoring, security, and testing alike.
* **[9arm (thananon)](https://github.com/thananon/9arm-skills)** — The structural backbone behind incident-response: the `debug-mantra` skill's reproduce → trace → falsify → cross-reference discipline and the `post-mortem` skill's engineer-audience write-up format.
* **[Andrej Karpathy](https://karpathy.ai)** — Credited directly in the coding-philosophy guidelines: "that app shouldn't exist" — the reminder that taste, judgment, and spec design remain the human bottleneck when agents can execute code.
