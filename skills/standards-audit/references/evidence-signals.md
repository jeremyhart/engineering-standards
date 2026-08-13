# Evidence Signals

Concrete signals to look for per control. Grade against the definition **at the required level** in
the section files — these signals tell you where to look, the section entry tells you what counts.

If a signal can only be verified outside the repo (source-host settings, cloud portal, human
process), mark the control `Unknown` and list what to check. Signals are illustrative, not
exhaustive — a project using a different-but-equivalent tool still counts.

## AI-Assisted Development

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Agent-readable project context | A context file at root (`CONTEXT.md`, `AGENTS.md`, `ARCHITECTURE.md`, `.github/copilot-instructions.md`) covering architecture, conventions and gotchas. Higher levels: only one file has content and others reference it; a domain/glossary section; links to decision records | Whether it is actually current |
| Agent configuration reviewed as code | Agent instructions, skills and hooks tracked in git (not gitignored, not global-only) | Whether PR review covers agent-config changes |
| Destructive-action guardrails | A "do not" / guardrails / boundaries section in the agent instructions; hook or wrapper config blocking destructive commands (recursive deletes, force pushes, production deletes) | Enforcement configured outside the repo |
| Automated post-edit checks | Hook config running lint/format after agent edits (`.github/hooks/*.json` `PostToolUse`, or equivalent) | — |
| Agent issue tracking | Tracker named in the agent instructions; triage label vocabulary defined (`.github/labels.yml`, `docs/agents/*`) | Tracker access for agents |
| Automated diagnosis skills | Skill files (`.agents/skills/`, `.claude/skills/`, `skills/`) that reference this repo's deploy targets, environments and log locations, and produce a diagnosis rather than a dump | Whether it is wired into the incident path |

## API & Contracts

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) | `N/A` if |
|---|---|---|---|
| Interface contract | Shared types or a generated client between client and server; OpenAPI/GraphQL schema or IDL; CI step diffing the contract for breaking changes | — | No API surface |
| Published API contract | Versioned routes or media types; a deprecation policy document with a stated window; consumer contract tests; changelog entries for API changes | Consumer communication practice | No consumers you can't deploy with |

## Code Quality

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Linting | Linter config (`.eslintrc*`, `ruff.toml`, `.rubocop.yml`, etc.); CI step running lint and failing on error; few blanket disables | — |
| Formatting | Formatter config (`.prettierrc*`, `[tool.black]`, etc. — `.editorconfig` alone doesn't count); CI step checking formatting | — |
| Type safety | Type checker configured and run in CI (`tsc --noEmit`, `mypy`, `pyright`); strictness settings raised beyond defaults at higher levels; escape hatches rare and commented | — |

`Type safety` is `N/A` where the language has no type checker available — cite that precondition.

## Continuous Integration

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| PR checks required before merge | Workflow running build+lint+types+tests on `pull_request` | Required-status-checks setting on the source host |
| Dependency update policy | Update bot config with a schedule (`dependabot.yml`, `renovate.json`); documented cadence and major-version approach | Whether update PRs are actually merged |
| Build provenance | Build tagged with its source commit; SBOM generation step (`syft`, `cyclonedx`); artifact signing (`cosign`, provenance attestation) | Published SBOM / signature verification by consumers |

## Database

*`N/A` unless the project has a database.*

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Database changes only through code | Versioned migration files (`migrations/`, `db/migrate/`, `prisma/migrations/`, `supabase/migrations/`); no console-applied changes referenced in docs. Higher levels: no standing human write access to production | Actual production access grants |
| Migrations in pipeline | Deploy workflow applies migrations before the app deploy, with no manual step | — |
| Migrations rehearsed before production | Workflow runs migrations against a non-production environment first | Whether the dataset is representative |
| Non-destructive migrations | Expand–contract visible in migration history (add, backfill, later drop); migration lint in CI (`squawk`, `atlas lint`) | Team practice |
| Forward-only migrations | No `down`/`revert` files, or the tool is configured to disallow them; documented recovery is redeploy-previous-version | — |

## Deployment

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Automated deployment | Deploy workflow triggered by push or tag; no deploys from developer machines; environment determined by trigger rather than manual input | Whether anyone still deploys by hand |
| Deployment validation | Post-deploy smoke test or health check in the workflow, failing the deploy on error | — |
| Rollback strategy | Runbook describing rollback; automated rollback step on failed validation at the top level | Whether it has been rehearsed |
| Artifact promotion | Build-once workflow promoting an artifact or image tag between environments; no rebuild per environment | — |
| Feature kill switches | Config/env flags gating risky features, or a flag SDK initialised | Flag service configuration |
| Automated versioning | `semantic-release`, `release-please`, `changesets` or equivalent in CI | — |
| Automated release notes | The same tools, or a CI step generating `CHANGELOG.md` on release | — |

## Developer Environment

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Setup docs and script | README setup section plus `scripts/setup.*` or a `setup` task that runs end to end | Whether it works on a clean machine |
| Standard task names | Task runner exposing `dev`, `build`, `test`, `lint`, `format`, `typecheck`; CI invoking the same entry points | — |
| Documented configuration | `.env.example` (or equivalent) listing every required variable, with descriptions and no real secret values | — |
| Validated configuration at startup | Config parsed and validated once at startup with clear errors (zod, pydantic-settings, Options + `ValidateOnStart`) | — |
| Pinned tool versions | Version files (`.nvmrc`, `.tool-versions`, `mise.toml`, `global.json`); lockfile committed; CI using the same versions | — |
| Seeding script | `scripts/seed.*`, `db/seeds/`, or a documented seed task | — |

## Development Workflow

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Spec-driven development | Specs in the repo (`docs/specs/`, `specs/`, or issue templates demanding acceptance criteria); PRs referencing a spec; spec changes accompanying behaviour changes in history | Whether the spec preceded the work |
| Test-driven development | Test and implementation files moving together in commit/PR history; diff-coverage gate in CI (`codecov` patch status, `diff-cover`); a documented red-green-refactor loop | Whether tests were literally written first |
| Spec-first agent workflow | Prompt/agent flow files describing align → spec → tasks → implement → review; briefs committed alongside agent work | Practice |

## Documentation

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| README completeness | Purpose and run in the first screenful; deploy section; architecture diagram or component description; operational surface at the top level | — |
| Architecture decision records | `docs/adr/`, `docs/decisions/` or `adr/` with numbered, dated records; linked from the project context | — |
| Infrastructure documentation | `docs/infrastructure.md` or generated-from-IaC docs describing topology, identities and access | Whether it matches reality |
| Docs drift detection | Scheduled workflow running a docs-vs-code check (link check, OpenAPI diff, or a drift skill); issues filed automatically from it | Whether filed issues get closed |

## Frontend

*`N/A` unless the project has a user-facing UI.*

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Accessibility baseline | Semantic markup and labelled controls; a11y lint rules; CI step running axe/Lighthouse/pa11y failing on critical violations | Manual keyboard/screen-reader passes |

## Infrastructure

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Infrastructure as code | `infra/`, `bicep/`, `terraform/`, `pulumi/` with resource definitions; remote state with locking; plan-on-PR workflow | Whether production matches the code (drift) |
| Hosting details documented | Docs recording account/subscription id, resource names, region, DNS and registrar, pipeline, third-party services, billing owner | — |
| Non-production environment | IaC or config defining a staging environment; deploy workflow targeting it before production; deployment slots configured | Whether it actually mirrors production |
| Environment parity | Environments provisioned from the same IaC with parameters; documented per-environment differences | Actual configuration drift between environments |

## Observability

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Structured logging | Logger configured for structured output (pino, structlog, Serilog); consistent fields; configurable level; shipping to a queryable sink | Retention configuration |
| Log hygiene | Redaction configured at the logger with a field deny-list; identifiers in path/body rather than query strings; documented retention window | Actual retention in the log platform |
| Request correlation | Request/trace id generated or read from a header, attached to every log line, propagated to outbound calls and background jobs | Tracing backend configuration |
| Error tracking | Error-tracker SDK initialised at the app entry point; errors grouped and triaged | DSN configured in production |
| Health endpoint | Route for `/health`, `/healthz`, `/readyz`; dependency checks inside it | External monitor calling it |
| Alerting | Alert rules defined in IaC; alerts referencing owners and runbooks | Alert routing, on-call integration, whether alerts are actionable |

`Log hygiene` is `N/A` where the project handles no personal data — cite that precondition.

## Operations

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Ownership and on-call | CODEOWNERS or docs naming the service owner; escalation path documented | On-call rotation tool |
| Incident response process | Doc describing alert → owner → fix → review, with severity levels | Rotation and postmortem practice |
| Runbooks | `docs/runbooks/` or `RUNBOOK.md` covering the known failure modes, linked from alerts | Whether they are exercised |
| Backup and restore | Backup configuration in IaC; restore procedure documented with a recorded restore time; RTO/RPO stated at the top level | Backup schedule, last successful restore test |
| Status communication | Status page config or documented notice channel | Whether it is used during incidents |
| Cost monitoring | Budget alerts defined in IaC | Cloud billing alerts and review cadence |

## Security

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Runtime secrets management | No secrets in the repo (secret scan clean); code reading from a managed store or platform-injected environment; workload identity rather than long-lived credentials | Managed identity / RBAC configuration; rotation |
| Secret scanning | CI running `gitleaks`/`trufflehog`, or host-native secret scanning config; baseline file with reasons and expiry dates | Host secret-scanning setting |
| Dependency vulnerability scanning | CI step running an audit tool; severity threshold blocking merge; suppression file with expiry; licence allowlist check | — |
| Least-privilege access | IaC role assignments scoped narrowly; separate service and human identities; roles documented | Actual role assignments; access reviews |
| Production access control | — | Cloud IAM, team membership, break-glass process and its logs |
| Deny-by-default auth | One central enforcement point applied globally, with public routes explicitly allowlisted; a new route protected without extra work; failures denying rather than allowing | Gateway configuration outside the repo |
| Endpoint authorization | Authorization declared per route (policies, decorators, guards) following one consistent approach; tenant isolation tests at the top level | Review practice |
| Secure coding patterns | **Sampled:** parameterised queries or ORM only (no string-built SQL); a validation library used at boundaries; output encoding by the template layer; SAST in CI | Penetration test cadence |
| Audit logging | Dedicated audit events for logins, permission changes and exports, written to a separate sink the app can't rewrite | Sink immutability and retention configuration |
| Personal data handling | Documented PII inventory; delete-user code path; retention enforced in schema/TTL/job; logs and backups accounted for | Whether deletion actually cascades in production |
| Rate limiting and abuse protection | Rate-limit middleware or binding; IaC defining gateway/WAF rules; per-user quotas on costly endpoints | Platform WAF/rate rules configured outside the repo |

`Personal data handling` is `N/A` where no personal data is held; `Rate limiting and abuse protection`
is `N/A` where the service is not reachable by untrusted parties. Cite the precondition either way.

## Source Control & Review

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Branch protection | — | Branch protection rules on the default branch; whether they apply to administrators |
| Code review process | CODEOWNERS; CONTRIBUTING describing review expectations | Required-reviewers setting; whether reviews happen |
| Pre-commit hooks | `.husky/`, `.pre-commit-config.yaml`, `lefthook.yml`, `lint-staged` config; installed by the setup script; fast checks only | — |
| Commit naming schema | `commitlint.config.*` or a CI step validating commit messages | — |
| PR template | `.github/PULL_REQUEST_TEMPLATE.md` or equivalent, prompting for what/why/testing | — |

## Testing

| Standard | In-repo signals | Out-of-repo (→ `Unknown`) | `N/A` if |
|---|---|---|---|
| Unit testing | Test files present and run in CI; per-test data isolation rather than shared mutable fixtures | — | — |
| Integration testing | Tests against real dependencies (testcontainers, a CI service database, real handlers); run in CI | — | No service boundaries |
| End-to-end testing | `e2e/` or Playwright/Cypress config; run in CI or on a schedule; run against production after deploy at the top level | — | Library with no user flows |
