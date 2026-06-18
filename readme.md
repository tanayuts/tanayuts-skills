# tanayuts

> Lazy but highly efficient Senior Engineer — YAGNI-focused, systematic debugging, strict PR scrutiny, full-stack toolkit.

A Claude skill set that turns Claude into a senior engineer who writes the minimum code that works and nothing more.

## What this is

A collection of 6 knowledge files that Claude reads on-demand. Each file activates a specific mode when you say the right trigger phrase. The root `SKILL.md` is the router — it tells Claude which file to load and when.

## Structure

```
tanayuts/
├── SKILL.md                      # Router — read this first
├── claude.md                     # Project config for this repo
├── readme.md                     # This file
├── coding-philosophy/SKILL.md    # Ponytail rules — always active
├── incident-response/SKILL.md    # Prod is down protocol
├── architecture-estimation/      # System design + estimation
├── security-testing/SKILL.md     # Security review + test plans
├── teaching-writing/SKILL.md     # Socratic mode + tech writing
└── refactoring-performance/      # Refactor guide + performance
```

## How to use

### In Claude Projects
1. Upload the entire `tanayuts/` folder to a Claude Project
2. Claude will automatically load `SKILL.md` as the skill
3. Start coding — Ponytail mode is always on

### Trigger phrases

| Say this... | Claude activates... |
|---|---|
| *(any code task)* | Ponytail full mode — YAGNI ladder |
| `ponytail-review` | Scan for over-engineering |
| `ponytail-audit` | Find bloat across the repo |
| `ponytail debt` | List deferred shortcuts |
| `prod is down` / `incident` | Real-time incident response |
| `design` / `architect` | System design with tradeoff analysis |
| `estimate` / `break down` | Task breakdown + T-shirt sizing |
| `security review` | Vulnerability checklist + fixes |
| `test plan` | Test level assignment + coverage |
| `teach me` / `socratic` | Guided learning through questions |
| `write a README` / `ADR` / `runbook` | Structured documentation |
| `refactor` / `restructure` | Safe refactoring with test-first |
| `it's slow` / `optimize` | Profile → bottleneck → measure → stop |

### Intensity controls
| Say this... | Effect |
|---|---|
| `ponytail lite` | Relax the YAGNI pressure slightly |
| `ponytail ultra` | Maximum ruthlessness |
| `stop ponytail` / `normal mode` | Disable Ponytail entirely |

## Philosophy

**Lazy = efficient, not careless.**

The best code is the code never written. Every line added is a line someone has to read, test, debug, and maintain. This skill set enforces that discipline across the full engineering lifecycle — from architecture down to the PR review.

What it never skips: input validation at trust boundaries, error handling that prevents data loss, security, accessibility, anything you explicitly ask for.

## Credits

This skill set stands on the shoulders of three people:

**[Ponytail (DietrichGebert)](https://github.com/DietrichGebert/ponytail)** — The core of every file here. The YAGNI ladder, the `ponytail:` comment convention, and the lite/full/ultra intensity levels run straight through coding-philosophy and get applied to architecture, refactoring, security, and testing alike. "The best code is the code you never wrote."

**[9arm (thananon)](https://github.com/thananon/9arm-skills)** — The structural backbone behind incident-response: the `debug-mantra` skill's reproduce → trace → falsify → cross-reference discipline and the `post-mortem` skill's engineer-audience write-up format trace back to 9arm's engineering operating system, even though the dedicated debugging-review and management-talk files aren't part of this particular bundle. (9arm's full set also includes `scrutinize` and `qwen-agent`.)

**[Andrej Karpathy](https://karpathy.ai)** — Credited directly in the coding-philosophy addendum: "that app shouldn't exist" — the reminder that most software shouldn't exist in the first place — and that taste, judgment, and spec design remain the human bottleneck when agents can do the execution.

The remaining files (architecture-estimation, security-testing, teaching-writing, refactoring-performance) are original write-ups built on the Ponytail philosophy as their guiding principle.

---

## Adding a new skill

1. Create a new folder: `your-skill-name/`
2. Add `SKILL.md` inside with the format: trigger conditions → behavior
3. Register it in the root `SKILL.md` under "When to read and use which knowledge file"
4. Add the trigger phrase to the skill menu table
