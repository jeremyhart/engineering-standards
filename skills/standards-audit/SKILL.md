---
name: standards-audit
description: 'Audit a codebase against the Software Project Standards and report which controls are met, missing, unknown, blocked, or not applicable. Use when asked to audit, assess, grade, or score a project against engineering or coding standards, or to check whether it meets the level its impact demands (Personal/Shared/Critical/Contracted).'
argument-hint: 'Optional: target level (Personal/Shared/Critical/Contracted) or category to focus on'
---

# Standards Audit

Audits a codebase against the standard and reports fit — what's in place, what's missing, what can't be checked from the repo alone, and what matters most given the project's level.

The standard ships inside this skill: resolve `standards.md` at [references/standards.md](./references/standards.md), the level model at [references/levels.md](./references/levels.md), per-control definitions under [references/sections/](./references/sections/), and stack facets under [references/stacks/](./references/stacks/). (In the source repo they instead live two levels up from this file — fall back to `../../standards.md`, `../../levels.md`, `../../sections/` and `../../stacks/` if the `references/` copies are absent.) Never fetch them from the network. If neither location exists, the skill was installed incompletely; tell the user to re-copy the full `standards-audit` skill folder.

**`standards.md` is only an index.** It names the controls; the section files define what each one means at each level. You cannot grade a control without reading its section entry.

## When to Use

- User asks to audit, assess, grade, or score a codebase against engineering or coding standards
- User asks "are we ready for prod" or similar readiness questions
- User asks which standards to tackle next given limited time or effort

## Procedure

### 1. Determine the required level

Read [references/levels.md](./references/levels.md) first — it defines the four levels and how to rate a project.

Use the argument if given. Otherwise **rate the project's impact** from the four questions in `levels.md` (disruption, disclosure, loss, obligation), taking the worst answer, using evidence from the repo: what the README says the project is, whether it serves external users, whether it holds personal or financial data, whether it's the system of record for anything, whether there are customers under agreement.

The required level is `max(impact, desired maturity)` — impact is the floor; a user asking to be graded higher raises it, never lowers it.

**Rate by consequence, not audience.** A one-person tool holding irreplaceable or sensitive data is `Critical`, not `Personal`.

**Identify, don't interrogate:** detect level, project type, and applicable facets yourself and state them as assumptions at the top of the report. Only pause to ask when a detection is genuinely ambiguous — conflicting signals, no facet Detect match on an obvious stack, or an impact rating the repo gives no evidence for. Never ask about things the repo already answers.

### 2. Detect stack, cloud/hosting, and offer live connections

Before auditing, identify the environment and try to connect to it — live access turns many `Unknown` rows into real findings and makes the report far more complete.

**a. Identify the stack and cloud/hosting.** From the repo, determine:

- **Stack facets** (language/framework/platform) — via the facet `**Detect:**` lines (step 4).
- **Source host** — GitHub/GitLab/etc., from the git remote (`git remote -v`).
- **Cloud / hosting provider** — from IaC (`bicep/`, `terraform/`, `pulumi/`), deploy workflows (`azure/login`, `aws-actions/*`, `google-github-actions/*`, `cloudflare/wrangler-action`), config files (`wrangler.toml`, `vercel.json`, `netlify.toml`, `fly.toml`, `app.yaml`, `serverless.yml`), and provider SDKs in dependencies.

State each detection at the top of the report.

**b. Offer to connect.** Many controls can only be verified outside the repo (GitHub branch protection, cloud IAM/RBAC, billing alerts, WAF rules, deployed config). Tell the user, up front and explicitly:

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

**c. Read the repo's live source-control properties.** When the source host is reachable, fetch and record for the report: visibility (private vs public — state it explicitly so an unintentionally public repo is obvious), default branch and its protection, secret scanning and push protection, Dependabot/vulnerability alerts, whether the repo is a fork, collaborator count, and CI workflow count. Distinguish **checked and absent** (API says off) from **not checked** (no access) in every finding.

**d. Record connection status.** Track, per service, whether you achieved live access: `connected`, `unavailable` (no MCP/CLI or not authenticated), or `declined` (user opted out). This drives the completeness flag in the report (step 7) and how out-of-repo rows are resolved (step 5).

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

Read [references/evidence-signals.md](./references/evidence-signals.md) — it maps each control to concrete in-repo signals and flags those that require out-of-repo verification.

Then detect applicable stack facets: read the `**Detect:**` line of each `stacks/<facet>.md` (resolved as above); for every facet that applies, use its sections to interpret the corresponding standards with stack-specific tooling (facet headings match standard names exactly).

**Committed-secrets sweep.** Explicitly check for configuration and secret material committed to the repository: `.env*` files (other than `*.example`/`*.sample`/`*.template`), private keys (`*.pem`, `id_rsa*`), and cloud credential files (`*credentials*.json`, `.npmrc`/`.pypirc` with tokens), in the working tree **and** in git history (`git log --diff-filter=A -- .env` etc.). A hit with real values feeds the grading of *Runtime secrets management* and *Documented configuration*, and — because a live credential in history is exploitable regardless of level arithmetic — always qualifies the finding for the Priority section.

**AI-generated code detection.** Look for signals that the codebase is substantially agent- or AI-generated, and flag it in the report as a factual finding (it is context, not a defect): committed agent workspaces (`.lovable/`, `.cursor/`, `.claude/`, `.github/copilot-instructions.md`), AI co-author trailers in commit history (`Co-Authored-By: Claude/Copilot`), platform auto-commit patterns (e.g. Lovable's uniform "Changes" messages), and unmodified generator boilerplate (template READMEs). When detected, name the tool and the evidence in the Technology detected section (an "AI code generation" row under Source control & toolchain), and weigh it when grading AI-Assisted Development controls — a repo built by agents is held to those controls with more force, not less.

Also inventory repo-level agent skills (`.agents/skills/`, `.claude/skills/`, `.github/skills/`, `skills/`) — these are evidence for `Automated diagnosis skills` and `Agent configuration reviewed as code` (AI-Assisted Development) and `Spec-first agent workflow` (Development Workflow). Ignore user-global skills (e.g. `~/.agents/skills/`): personal machine state, not codebase quality.

### 5. Audit each control

For every control in `standards.md`, read its entry in the relevant `sections/` file and grade it **against the definition at the required level** — not against the highest level, and not against a general impression of the control.

Assign one status:

- **`Met`** — the definition at the required level is satisfied
- **`Partial`** — some of that level's definition is satisfied but not all (e.g. linter configured but not enforced in CI); note which part is missing, and name the level actually reached
- **`Missing`** — nothing of the required level's definition is in place, and it is verifiable from the repo
- **`Unknown`** — requires out-of-repo verification that could not be performed; list exactly what to check
- **`Blocked`** — the control has a `Requires:` prerequisite that is `Missing` or `Partial`. Report the prerequisite as the actionable gap and do **not** recommend this control until it is met — implementing it first is worse than not implementing it (e.g. automated rollback over destructive migrations)
- **`N/A`** — an `Applies when:` precondition does not hold, or the conditional category's precondition does not hold. **Always cite the precondition that failed** — never mark N/A without one

Also record the level each control actually reaches, where it's higher or lower than required — that is what lets the report show distance rather than a binary.

For controls requiring out-of-repo verification, use the live connections from step 2:

- If the relevant service is **connected**, actually perform the check (read-only) and assign a real status from what you find — don't default to `Unknown`.
- If the service is **unavailable** or **declined**, mark it `Unknown` and name the specific setting to check plus how to enable the connection.

**Sampled controls.** Some controls (notably `Secure coding patterns`) cannot be exhaustively verified. Sample the evidence — parameterised queries rather than string-built SQL, a validation library used at boundaries, output encoding by default — and state in the report that the finding is based on sampling rather than exhaustive verification.

Do not silently skip controls. Every control in `standards.md` must get one of these statuses.

### 6. Filter and rank recommendations

Recommendations are drawn **only** from `Missing`, `Partial` and prerequisite-of-`Blocked` controls that are below the required level. A control whose definition begins above the required level is not a gap — it isn't expected yet; report those separately so the user isn't pushed to gold-plate.

Rank by:

1. **Distance below the required level** — a control at nothing when level 3 is required outranks one sitting at level 2 of 3
2. **Prerequisites first** — a control that other controls declare as `Requires:` outranks its dependants
3. **Effort at the required level** ascending (`S` > `M` > `L`), read from that level's row in the section file

Take the top 3-5.

### 7. Assemble the report data

Collect every finding into one row list and derive **all** counts from it. Every number that appears anywhere in the output — lede sentences, chart segments, filter counts, section headings, footer — must reconcile against that single list; before writing, re-count any sentence that states a quantity ("two gaps could…") against the rows it summarises. A stated count that disagrees with its list is a defect, not a style issue.

Also assemble:

- **Standards version** — read `VERSION` (resolved like the other references) and state it in the header and footer, so the report traces to the exact set of controls in force.
- **Repo address** — the actual source-control address from the git remote (e.g. `github.com/org/project`), linked in the header and footer.
- **Technology detected** — concrete named technologies only (language, frameworks, database, hosting, source control, integrations, facets applied). Name the actual technology/SDK/service precisely or omit it; never vague entries like "email provider".
- **Assumptions** — everything the findings are contingent on: how the level was inferred, services not reached and what that clouds, what was sampled rather than exhaustively verified, anything taken on trust.
- **Effort words** — render effort as **Small / Medium / Large** in report output (the section files' S/M/L codes are source data, not display text).

**Completeness flag.** Report the connection status from step 2. If any detected service is `unavailable` or `declined`, state prominently that **the report is incomplete**: name which services were not reached, and note that the `Unknown` rows tied to them could not be verified and may hide gaps. Only call the audit complete when every detected out-of-repo service was connected.

**Progress updates.** An audit is long — keep the user oriented throughout: announce each phase as you enter it (level rating, connections, control grading by category, report writing) and surface notable findings as you hit them, rather than going quiet until the end.

### 8. HTML report — the deliverable

The report **is** the HTML file. Do not print a markdown report first — produce the HTML, tell the user where it was written, and **open it in their browser** (`open`/`xdg-open`/`start`, whichever the platform has; if none works, say so and give the `file://` path). Produce the markdown format below **only when the user explicitly asks for markdown**.

Write a self-contained `standards-audit.html` at the repo root (inline CSS and JS, no external assets — must open cleanly from `file://`).

**Do not author the HTML/CSS from scratch, and do not invent class names, spacing, or colours.** [references/example-report.html](./references/example-report.html) is the canonical output, not a mood board. Build the report by copying it verbatim and substituting content:

1. Read the full contents of [references/example-report.html](./references/example-report.html).
2. Copy it byte-for-byte as your starting point — the entire `<style>` block, every class name, every `<div>` nesting level, the toggle/tab `<script>`, all of it, unchanged.
3. Edit only the **content nodes** (text inside tags, table rows, card lists, counts) to reflect this audit's data. Never rename a class, restructure a section's DOM, change a colour token, alter padding/grid values, or add new CSS rules. If a section template repeats (e.g. a `.priority-item`, a table `<tr>`, an `.unknown-card`), clone that exact block for each additional row and only edit its text.
4. Add or remove whole `<section>` blocks only where the example itself says to (e.g. omit `01 · Priority` when there are no priority gaps, omit `04b` when nothing was verified live) — never restyle a kept section.
5. Before writing the file, diff your output against the example's `<style>` block in your head: if you cannot point to the exact example line a CSS rule came from, delete it.

[references/html-report.md](./references/html-report.md) explains *which data goes in which section* — use it only to know what content to substitute, never as a spec to re-derive markup from. It also defines the chart substitutions (coverage bar, level meter, source pills), the gap-row expansions (verbatim now-vs-required level definitions + copy-able fix prompt), the status filter, and the full-width fallback for empty columns — none of these are to be re-invented per report.

Section order (all copied structurally from the example): header + lede + hero visuals → `01 · Priority` (a synthesis — the shared risk, why together, the fix order; never a re-listing of table rows; omit if no priority gaps) → `02 · Next steps` → `03 · All gaps` (grouped by category, filterable, expandable) → `04 · Unverified` → `04b · Verified against live infrastructure` (the live repo/cloud properties from step 2c; omit if nothing verified live) → `05 · In good standing` → `06 · Technology detected` → deferred/N-A → `07 · The case for improvement` → `08 · Assumptions` → footer.

Investment case (`07 · The case for improvement`) only when there are actionable gaps; see step 9.

### 9. Investment case

When there are actionable gaps, include the investment case (the HTML report's `07 · The case for improvement`; include it in the markdown report too when one is requested). Format it as real prose — short paragraphs for the business pitch, an ordered list of first actions for the engineering pitch — never a single unformatted block. Its purpose is to arm the developer to argue for codebase investment when it competes with product features. Two pitches, each ≤ 150 words, generated from the actionable gap list using the project's level and the per-level effort from the section files:

- **Business case** (primary — written to be handed to a non-technical stakeholder): translate gaps into business outcomes, taking the consequence from the level the project was rated at and *why* it was rated there — a `Critical` rating means the business stops or something irreplaceable is lost, and the gaps are what stand between the project and that. No tool or standard names. Contrast the cost of fixing now (sum of the effort at the required level, in rough days) with the compounding cost of deferring. Explicitly counter "it's not a feature": this work is what keeps features shipping at the current pace.
- **Technical case** — for peers and leads: name the controls, the level each currently reaches versus the level required, the tools, and the first action for each, plus why now (retrofitting cost grows with the codebase).

Do not inflate. If the project rates `Personal` or `Shared` and the gaps are small, say so honestly — the case is about pace, not catastrophe. Never argue from a consequence the rating doesn't support.

## Output Format (markdown — only when explicitly requested)

```markdown
## Standards Audit — <project> (type: <type>, level: <level>)

**Level:** <n · Name> — <the answer that set it, e.g. "the only copy of order records">
**Repo:** <linked source-control address> · **Standards:** v<VERSION> · **Date:** <date>
**Stack:** <facets> · **Source host:** <host> · **Cloud/hosting:** <provider>
**Live access:** GitHub <connected|unavailable|declined> · <cloud> <connected|unavailable|declined>
**Completeness:** <Complete | ⚠️ Incomplete — <services> not reached; Unknown controls tied to them unverified>

**Coverage:** X Met · Y Partial · Z Missing · B Blocked · U Unknown · N N/A (out of T controls)

### Gaps to act on (below the required level)
| Status | Standard | Category | Reaches | Required | Notes | Effort |
|---|---|---|---|---|---|---|
| Missing | ... | ... | — | 3 | ... | M |

### Blocked (prerequisite not met)
| Standard | Blocked by | Why it matters |
|---|---|---|

### Not yet expected at this level
Informational — no action needed unless the project's impact rises.
| Standard | Category | First expected at |
|---|---|---|

### Requires out-of-repo verification (Unknown)
| Standard | Category | What to check |
|---|---|---|
| ... | ... | ... |

### Not applicable
| Standard | Failed precondition |
|---|---|

### Recommended next steps
1. **<item>** — <one-line why + concrete first action>
2. ...
```

## Notes

- If the user scoped the request to a single category, only audit that category but still detect project type and produce the coverage line for that subset.
- If `standards.md` has an `## Unsorted` section, audit its controls like any other category.
- Cite controls as *Category N* (e.g. *Security 4*) where a precise reference helps — standards are numbered within their category.
- If an evidence signal in [references/evidence-signals.md](./references/evidence-signals.md) is missing for a row, fall back to the row's description in `standards.md` and note the gap in the report so signals can be added later.
