---
name: standards-audit
description: 'Audit a codebase against the engineering-standards checklist and report which items are met, missing, unknown, or not applicable. Use when asked to audit, assess, grade, or score a project against engineering or coding standards, or to check readiness for a given project stage (Prototype/Internal/Prod).'
argument-hint: 'Optional: target stage (Prototype/Internal/Prod) or category to focus on'
---

# Standards Audit

Audits a codebase against the checklist in `standards.md` and reports fit — what's in place, what's missing, what can't be checked from the repo alone, and what matters most given the project's stage.

The checklist and stack facets ship inside this skill: resolve `standards.md` at [references/standards.md](./references/standards.md) and facets under [references/stacks/](./references/stacks/). (In the engineering-standards source repo they instead live two levels up from this file — fall back to `../../standards.md` and `../../stacks/` if the `references/` copies are absent.) Never fetch them from the network. If neither location exists, the skill was installed incompletely; tell the user to re-copy the full `standards-audit` skill folder.

## When to Use

- User asks to audit, assess, grade, or score a codebase against engineering or coding standards
- User asks "are we ready for prod" or similar stage-readiness questions
- User asks which standards to tackle next given limited time or effort

## Procedure

### 1. Determine target stage

Use the argument if given; otherwise infer from context (e.g. a repo serving real users is at least `Prod`). Stage order: `Prototype` < `Internal` < `Prod`.

**Identify, don't interrogate:** detect target stage, project type, and applicable facets yourself and state them as assumptions at the top of the report. Only pause to ask the user when a detection is genuinely ambiguous — conflicting signals, no facet Detect match on an obvious stack, or a target stage that can't be inferred. Never ask about things the repo already answers.

### 2. Detect project type

Look at top-level files to classify the project — this gates which categories apply:

| Signal | Type | Categories to mark `N/A` |
|---|---|---|
| `package.json` with only `main`/`exports`, no server code | Library | Observability, most of CI deploy, Database (unless present) |
| `Dockerfile` + web framework, or `serverless.yml`, or Azure Functions config | Service | none |
| Only `bin/` or CLI entry point | CLI tool | Observability (health/alerting), Database (unless present) |
| Only `infra/`, `bicep/`, `terraform/` | Infra-only | Code Quality (partially), Testing (unit only) |

Also apply the *"Applies when: ..."* precondition on conditional categories in `standards.md` — if it doesn't hold, mark the whole category `N/A` with the reason.

Record the detected type in the report; confirm with the user only if the signals conflict.

### 3. Load evidence signals and stack facets

Read [references/evidence-signals.md](./references/evidence-signals.md) — it maps each checklist row to concrete in-repo signals and flags items that require out-of-repo verification.

Then detect applicable stack facets: read the `**Detect:**` line of each `stacks/<facet>.md` (resolved as above); for every facet that applies, use its sections to interpret the corresponding standards with stack-specific tooling (facet headings match standard names exactly).

Also inventory repo-level agent skills (`.agents/skills/`, `.claude/skills/`, `.github/skills/`, `skills/`) — these are evidence for AI-Assisted Development rows (Automated diagnosis skills, Agent config reviewed as code, Spec-first agent workflow). Ignore user-global skills (e.g. `~/.agents/skills/`): personal machine state, not codebase quality.

### 4. Audit each row

For every row in `standards.md` (resolved as above), assign one status:

- **`Met`** — all in-repo signals for the item are present
- **`Partial`** — some signals present but not all (e.g. linter configured but not enforced in CI); note which are missing
- **`Missing`** — no in-repo signals found, and the item is verifiable from the repo
- **`Unknown`** — item requires out-of-repo verification (GitHub settings, cloud portal, human process); list exactly what to check
- **`N/A`** — item doesn't apply based on step 2 project type; state why

Do not silently skip rows. Every row in `standards.md` must get one of these statuses.

### 5. Filter and rank recommendations

Recommendations are drawn **only** from `Missing` and `Partial` rows where `Min stage` ≤ target stage. Rows failing above the target stage are not gaps — they're not yet expected; report them separately so the user isn't pushed to gold-plate.

Rank by:

1. `Risk if absent` severity: `Causes breach` > `Causes incidents` > `Slows team` > `Polish`
2. `Effort` asc: `S` > `M` > `L`

Take the top 3-5.

### 6. Report

Use the format below. Include the coverage line so the user can see how much of the audit is `Unknown` vs. actually checked.

### 7. HTML report

Also write a self-contained `standards-audit.html` at the repo root (inline CSS and JS, no external assets — must open cleanly from `file://`). Follow the template in [references/html-report.md](./references/html-report.md), and mirror the worked example at [references/example-report.html](./references/example-report.html):

- **Lede** — a plain-language finding (met-of-applicable, sharpest risk, rough total effort) plus a Met/Partial/Missing/Unknown stat strip. No banner colour and no grade — state stage and coverage in prose.
- **Numbered sections, urgency-ordered** — `01 · Priority` (breach-risk gaps, red; omit if none) → `02 · Next steps` → `03 · All gaps` (table) → `04 · Unverified` → `05 · In good standing` → deferred/N-A → `06 · Why now`.
- **Detail content** mirrors the markdown report — same rows and ordering.
- **Investment case** (`06 · Why now`) — only when there are actionable gaps; see below.

Tell the user where the file was written.

### 8. Investment case

When there are actionable gaps, include an **Investment case** section (in the HTML report; offer it at the end of the markdown report too). Its purpose is to arm the developer to argue for codebase investment when it competes with product features. Two pitches, each ≤ 150 words, generated from the actionable gap list using the Risk and Effort columns:

- **Business case** (primary — written to be handed to a non-technical stakeholder): translate gaps into business outcomes — outage risk, breach exposure, delivery slowdown. No tool or standard names. Contrast the cost of fixing now (sum of Effort, in rough days) with the compounding cost of deferring. Explicitly counter "it's not a feature": this work is what keeps features shipping at the current pace.
- **Technical case** — for peers and leads: name the rows, the tools, and the first action for each, and why now (retrofitting cost grows with the codebase).

Do not inflate: if the actionable gaps are all `Slows team`/`Polish`, say so honestly — the case is about pace, not risk.

## Output Format

```markdown
## Standards Audit — <project> (type: <type>, target stage: <stage>)

**Coverage:** X Met · Y Partial · Z Missing · U Unknown · N N/A (out of T items)

### Gaps to act on (Missing or Partial, Min stage ≤ target)
| Status | Item | Category | Risk | Notes | Effort |
|---|---|---|---|---|---|
| Missing | ... | ... | ... | ... | ... |

### Not yet expected at this stage (Missing or Partial, Min stage > target)
Informational — no action needed until the project advances a stage.
| Status | Item | Category | Min stage | Effort |
|---|---|---|---|---|

### Requires out-of-repo verification (Unknown)
| Item | Category | What to check |
|---|---|---|
| ... | ... | ... |

### Not applicable
| Item | Reason |
|---|---|

### Recommended next steps
1. **<item>** — <one-line why + concrete first action>
2. ...
```

## Notes

- If the user scoped the request to a single category, only audit that category but still detect project type and produce the coverage line for that subset.
- If `standards.md` has an `## Unsorted` section, audit its rows like any other category.
- If an evidence signal in [references/evidence-signals.md](./references/evidence-signals.md) is missing for a row, fall back to the row's description in `standards.md` and note the gap in the report so signals can be added later.
