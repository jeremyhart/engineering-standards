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

### 2. Detect stack, cloud/hosting, and offer live connections

Before auditing, identify the environment and try to connect to it — live access turns many `Unknown` rows into real findings and makes the report far more complete.

**a. Identify the stack and cloud/hosting.** From the repo, determine:

- **Stack facets** (language/framework/platform) — via the facet `**Detect:**` lines (step 4).
- **Source host** — GitHub/GitLab/etc., from the git remote (`git remote -v`).
- **Cloud / hosting provider** — from IaC (`bicep/`, `terraform/`, `pulumi/`), deploy workflows (`azure/login`, `aws-actions/*`, `google-github-actions/*`, `cloudflare/wrangler-action`), config files (`wrangler.toml`, `vercel.json`, `netlify.toml`, `fly.toml`, `app.yaml`, `serverless.yml`), and provider SDKs in dependencies.

State each detection at the top of the report.

**b. Offer to connect.** Many checklist rows can only be verified outside the repo (GitHub branch protection, cloud IAM/RBAC, billing alerts, WAF rules, deployed config). Tell the user, up front and explicitly:

> Connecting me to your source host and cloud/hosting provider would make this report much more relevant and complete — I can verify settings that aren't visible in the repo instead of leaving them Unknown.

Then, for each detected service, prefer an **MCP server** if one is available; fall back to the provider **CLI** if not. Suggested connections by provider:

| Detected service | Preferred: MCP server | Fallback: CLI |
|---|---|---|
| GitHub | GitHub MCP server (`get_me`, branch-protection/rulesets, required checks, secret scanning, Dependabot) | `gh` |
| GitLab | GitLab MCP server | `glab` |
| Azure | Azure MCP server | `az` |
| AWS | AWS MCP server | `aws` |
| Google Cloud | GCP MCP server | `gcloud` |
| Cloudflare | Cloudflare MCP server / API | `wrangler`, Cloudflare API |
| Vercel | Vercel MCP server | `vercel` |
| Supabase | Supabase MCP server | `supabase` |

Check availability of the relevant tools; if a needed MCP tool or CLI isn't present or isn't authenticated, ask the user to enable/authenticate it (e.g. `gh auth login`, `az login`). Don't run any state-changing commands — read-only verification only.

**c. Record connection status.** Track, per service, whether you achieved live access: `connected`, `unavailable` (no MCP/CLI or not authenticated), or `declined` (user opted out). This drives the completeness flag in the report (step 7) and how out-of-repo rows are resolved (step 5).

### 3. Detect project type

Look at top-level files to classify the project — this gates which categories apply:

| Signal | Type | Categories to mark `N/A` |
|---|---|---|
| `package.json` with only `main`/`exports`, no server code | Library | Observability, most of CI deploy, Database (unless present) |
| `Dockerfile` + web framework, or `serverless.yml`, or Azure Functions config | Service | none |
| Only `bin/` or CLI entry point | CLI tool | Observability (health/alerting), Database (unless present) |
| Only `infra/`, `bicep/`, `terraform/` | Infra-only | Code Quality (partially), Testing (unit only) |

Also honour the italic precondition note under conditional category headings in `standards.md` (e.g. *"Only relevant if the project has a user-facing UI."*) — if it doesn't hold, mark the whole category `N/A` with the reason.

Record the detected type in the report; confirm with the user only if the signals conflict.

### 4. Load evidence signals and stack facets

Read [references/evidence-signals.md](./references/evidence-signals.md) — it maps each checklist row to concrete in-repo signals and flags items that require out-of-repo verification.

Then detect applicable stack facets: read the `**Detect:**` line of each `stacks/<facet>.md` (resolved as above); for every facet that applies, use its sections to interpret the corresponding standards with stack-specific tooling (facet headings match standard names exactly).

Also inventory repo-level agent skills (`.agents/skills/`, `.claude/skills/`, `.github/skills/`, `skills/`) — these are evidence for AI-Assisted Development rows (Automated diagnosis skills, Agent config reviewed as code, Spec-first agent workflow). Ignore user-global skills (e.g. `~/.agents/skills/`): personal machine state, not codebase quality.

### 5. Audit each row

For every row in `standards.md` (resolved as above), assign one status:

- **`Met`** — all in-repo signals for the item are present
- **`Partial`** — some signals present but not all (e.g. linter configured but not enforced in CI); note which are missing
- **`Missing`** — no in-repo signals found, and the item is verifiable from the repo
- **`Unknown`** — item requires out-of-repo verification that could not be performed; list exactly what to check
- **`N/A`** — item doesn't apply based on step 3 project type; state why

For rows requiring out-of-repo verification, use the live connections from step 2:

- If the relevant service is **connected**, actually perform the check (read-only) and assign `Met` / `Partial` / `Missing` from what you find — don't default to `Unknown`.
- If the service is **unavailable** or **declined**, mark the row `Unknown` and name the specific setting to check plus how to enable the connection.

Do not silently skip rows. Every row in `standards.md` must get one of these statuses.

### 6. Filter and rank recommendations

Recommendations are drawn **only** from `Missing` and `Partial` rows where `Min stage` ≤ target stage. Rows failing above the target stage are not gaps — they're not yet expected; report them separately so the user isn't pushed to gold-plate.

Rank by:

1. `Risk if absent` severity: `Causes breach` > `Causes incidents` > `Slows team` > `Polish`
2. `Effort` asc: `S` > `M` > `L`

Take the top 3-5.

### 7. Report

Use the format below. Include the coverage line so the user can see how much of the audit is `Unknown` vs. actually checked.

**Completeness flag.** Report the connection status from step 2. If any detected service is `unavailable` or `declined` (i.e. GitHub and/or the cloud/hosting provider could not be accessed), state prominently that **the report is incomplete**: name which services were not reached, and note that the `Unknown` rows tied to them could not be verified and may hide gaps. Only call the audit complete when every detected out-of-repo service was connected. Put this flag in both the markdown coverage block and the HTML lede.

### 8. HTML report

Also write a self-contained `standards-audit.html` at the repo root (inline CSS and JS, no external assets — must open cleanly from `file://`).

**Do not author the HTML/CSS from scratch, and do not invent class names, spacing, or colours.** [references/example-report.html](./references/example-report.html) is the canonical output, not a mood board. Build the report by copying it verbatim and substituting content:

1. Read the full contents of [references/example-report.html](./references/example-report.html).
2. Copy it byte-for-byte as your starting point — the entire `<style>` block, every class name, every `<div>` nesting level, the toggle/tab `<script>`, all of it, unchanged.
3. Edit only the **content nodes** (text inside tags, table rows, card lists, counts) to reflect this audit's data. Never rename a class, restructure a section's DOM, change a colour token, alter padding/grid values, or add new CSS rules. If a section template repeats (e.g. a `.priority-item`, a table `<tr>`, an `.unknown-card`), clone that exact block for each additional row and only edit its text.
4. Add or remove whole `<section>` blocks only where the example itself says to (e.g. omit `01 · Priority` when there are no breach-risk gaps, omit `04b` when nothing was verified live) — never restyle a kept section.
5. Before writing the file, diff your output against the example's `<style>` block in your head: if you cannot point to the exact example line a CSS rule came from, delete it.

[references/html-report.md](./references/html-report.md) explains *which data goes in which section* — use it only to know what content to substitute, never as a spec to re-derive markup from.

Section order (all copied structurally from the example): `01 · Priority` (breach-risk gaps, red; omit if none) → `02 · Next steps` → `03 · All gaps` (table) → `04 · Unverified` → `04b · Verified against live infrastructure` (omit if nothing verified live) → `05 · In good standing` → deferred/N-A → `06 · Why now`.

Detail content mirrors the markdown report — same rows and ordering. Investment case (`06 · Why now`) only when there are actionable gaps; see step 9.

Tell the user where the file was written.

### 9. Investment case

When there are actionable gaps, include an **Investment case** section (in the HTML report; offer it at the end of the markdown report too). Its purpose is to arm the developer to argue for codebase investment when it competes with product features. Two pitches, each ≤ 150 words, generated from the actionable gap list using the Risk and Effort columns:

- **Business case** (primary — written to be handed to a non-technical stakeholder): translate gaps into business outcomes — outage risk, breach exposure, delivery slowdown. No tool or standard names. Contrast the cost of fixing now (sum of Effort, in rough days) with the compounding cost of deferring. Explicitly counter "it's not a feature": this work is what keeps features shipping at the current pace.
- **Technical case** — for peers and leads: name the rows, the tools, and the first action for each, and why now (retrofitting cost grows with the codebase).

Do not inflate: if the actionable gaps are all `Slows team`/`Polish`, say so honestly — the case is about pace, not risk.

## Output Format

```markdown
## Standards Audit — <project> (type: <type>, target stage: <stage>)

**Stack:** <facets> · **Source host:** <host> · **Cloud/hosting:** <provider>
**Live access:** GitHub <connected|unavailable|declined> · <cloud> <connected|unavailable|declined>
**Completeness:** <Complete | ⚠️ Incomplete — <services> not reached; Unknown rows tied to them unverified>

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
