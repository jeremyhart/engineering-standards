# Evidence Signals

Concrete signals to look for per checklist item. For each item, `Met` requires **all** listed sub-conditions unless noted. If a sub-condition can only be verified outside the repo (GitHub settings, cloud portal, human process), mark the item `Unknown` and list what to check.

Signals are illustrative, not exhaustive — a project using a different-but-equivalent tool still counts as `Met`.

## AI-Assisted Development

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| CONTEXT.md | `CONTEXT.md` or equivalent (`ARCHITECTURE.md`) at root; contains architecture + conventions + gotchas sections | — |
| AGENTS.md | `AGENTS.md` or `.github/copilot-instructions.md` at root | — |
| Single source of truth for agent docs | Only one of AGENTS.md / CLAUDE.md / copilot-instructions.md has content; others are stubs/symlinks pointing to it | — |
| Domain model in CONTEXT.md | CONTEXT.md has a "Domain" / "Glossary" / "Ubiquitous language" section | — |
| ADRs agent-readable | `docs/adr/`, `docs/decisions/`, or `adr/` folder exists; referenced from CONTEXT.md/AGENTS.md | — |
| Agent config reviewed as code | AGENTS.md/skills/hooks tracked in git (not gitignored); no agent config in personal/global-only locations | PR review actually covers agent-config changes |
| Agent guardrails | AGENTS.md or copilot-instructions.md has a "do not" / "guardrails" / "boundaries" section | — |
| Destructive-command guardrails as a hook | `.github/hooks/*.json` with `PreToolUse` blocking `rm -rf`, `git push --force`, prod deletes | — |
| Post-tool-use hooks | `.github/hooks/*.json` with `PostToolUse` running lint/format/typecheck | — |
| Issue tracker + triage vocabulary for agents | AGENTS.md notes where issues live; `docs/agents/issue-tracker.md` / `docs/agents/triage-labels.md` or `.github/labels.yml`; triage-role labels defined (`needs-triage`, `ready-for-agent`, …) | Issue tracker access for agents |
| Spec-first agent workflow | `.github/prompts/` or `.github/agents/` contains align/spec/tickets/implement/review flow | — |
| Automated diagnosis skills | Skill files (`.agents/skills/`, `skills/`) that reference the repo's deploy targets/environments and diagnose failures | — |

## Source Control

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Branch protection | — | GitHub branch protection rules on default branch |
| Code review process | `CODEOWNERS` file in `.github/` or root; CONTRIBUTING.md mentions reviewer count | GitHub required-reviewers setting |
| Pre-commit hooks | `.husky/`, `.pre-commit-config.yaml`, `lefthook.yml`, or `package.json` `lint-staged` config | — |
| Commit naming schema | `commitlint.config.*`, `.commitlintrc*`, or CI workflow validating commit messages | — |
| PR template | `.github/PULL_REQUEST_TEMPLATE.md` (any case) or `.github/pull_request_template/` | — |

## CI

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| PR checks required before merge | `.github/workflows/*.yml` runs build+lint+typecheck+test on `pull_request` | GitHub required-status-checks setting |
| Secret scanning | CI workflow running `gitleaks`, `trufflehog`, or GitHub secret scanning enabled via `.github/` config | GitHub advanced security setting |
| Dependency vuln scanning | `.github/dependabot.yml`, `renovate.json`, or CI step running `npm audit` / `pip-audit` / `snyk` | — |
| Dependency update policy | CONTRIBUTING.md or docs describe cadence; Renovate/Dependabot config sets schedule | — |
| Docs drift detection | CI step running docs-lint / link-check / OpenAPI-diff and failing on drift, and/or a skill that checks docs against code | — |

## Deployment

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Continuous deployment | Deploy workflow triggered on push to main/tag; no `workflow_dispatch`-only deploys | — |
| Deployment validation | Deploy workflow runs smoke test / health check after deploy and fails on error | — |
| Rollback strategy | Runbook doc (`docs/runbooks/`, `RUNBOOK.md`) describes rollback | Tested-in-practice claim |
| Automated rollback on failed smoke test | Deploy workflow has post-deploy check that triggers rollback step on failure | — |
| Guarded release/deploy wrappers | Deploy scripts in `scripts/` or `Makefile` wrap raw deploy CLI commands | — |
| Promotion path defined by trigger | Deploy workflow environment gated on branch/tag pattern, not manual input | — |
| Artifact promotion | Workflow builds once, promotes via artifact/registry tag; no rebuild per environment | — |
| Feature kill switches | Env-var/config flags gating risky features, or flag SDK (LaunchDarkly, Flagship, Unleash) initialized | Flag service config |
| Automated versioning | `semantic-release`, `release-please`, `changesets`, or similar in CI | — |
| Automated release notes | Same tools as above, or CI step generating `CHANGELOG.md` on release | — |

## Infrastructure

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| IaC | `infra/`, `bicep/`, `terraform/`, `pulumi/` folder with resource definitions | Whether prod actually matches IaC (drift) |
| Hosting details documented | README or `docs/` records subscription/project ID, resource group, region, resource names | — |
| Environment parity | IaC uses parameterized environments; docs list per-env differences | — |
| Backup & restore procedure | Runbook documents restore steps | Backup schedule + last successful restore test |

## Security

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Runtime secrets management | No secrets in repo (gitleaks clean); code reads from a managed secret store or platform-injected env | Managed identity / RBAC config in cloud |
| Least-privilege access | IaC role assignments scoped narrowly; roles documented in `docs/` | Actual role assignments in cloud |
| Prod access control | — | Cloud IAM / GitHub team membership |
| Auth gates deny by default | Central auth middleware/gateway applied globally; public routes explicitly allowlisted | Gateway config outside repo |
| Authorization reviewed per endpoint | Authz rules declared per route (decorators, policy files); review noted in PR template/checklist | Review practice |
| Secure coding baseline | Input validation at boundaries (zod/pydantic/etc.); parameterised queries or ORM only; output encoding via framework | — |
| Audit logging | Code paths for logins/permission changes/data exports emit dedicated audit events to an append-only sink | Sink immutability config |
| Personal data handling | Docs list PII fields and where they live; delete-user code path exists; retention enforced via schema/TTL/cron | Whether deletion actually cascades in prod |
| Rate limiting & abuse protection | Rate-limit middleware/binding in code, or IaC defines gateway/WAF rate rules; per-user quota logic for costly endpoints | Platform-level WAF/rate rules configured outside repo |

## Operations

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Ownership & on-call | CODEOWNERS or `OWNERS` file; README/docs name service owner | On-call rotation tool |
| Incident response process | Doc describing alert → owner → fix → postmortem path (minimum: "who to ping, where to triage" note) | Rotation/postmortem practice |
| Runbooks | `docs/runbooks/` or `RUNBOOK.md` covering known failure modes | — |
| Status communication | Status page config/link or documented notice channel | Whether it's actually used during incidents |
| Cost monitoring | IaC defines budget alerts | Cloud billing alerts + review cadence |

## Code Quality

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Linting | Linter config (`.eslintrc*`, `ruff.toml`, `.rubocop.yml`, etc.); CI step runs lint and fails on error | — |
| Formatting | Formatter config (`.prettierrc*`, `pyproject.toml` `[tool.black]`, `.editorconfig` alone doesn't count); CI step runs formatter check | — |
| Typecheck | `tsconfig.json` with `"strict": true` (or per-lang equivalent); CI step runs `tsc --noEmit` / `mypy` / `pyright` | — |
| API contract defined | OpenAPI/GraphQL schema file, tRPC/Hono RPC router types shared with client, or generated client package; CI step diffing contract for breaking changes | — |

## Frontend

*Category is `N/A` unless the project has a user-facing UI.*

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Accessibility baseline | CI step running axe/Lighthouse/pa11y and failing on critical violations; a11y lint rules (eslint-plugin-jsx-a11y or equivalent) | Manual keyboard/screen-reader pass practice |

## Database

| Item | In-repo signals | Out-of-repo (→ `Unknown`) | `N/A` if |
|---|---|---|---|
| Migrations | `migrations/`, `db/migrate/`, `prisma/migrations/`, or ORM migration folder with versioned files | — | No database in project |
| Migrations in pipeline | Deploy workflow runs migration step before app deploy | — | As above |
| Migrations rehearsed on staging | Deploy workflow runs migrations against staging env before prod | — | As above |
| Non-destructive migrations | Migration review checklist / lint (`squawk`, `atlas-lint`) in CI | Team practice | As above |
| Forward-only migrations | Migration tool config disables `down` migrations, or convention documented | — | As above |

## Developer Environment

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Setup docs & script | README "Setup" section + `scripts/setup.*` or `Makefile setup` target that runs end-to-end | — |
| Standard task names | Task runner (e.g. `package.json` `scripts`, `Makefile`, `justfile`) includes `dev`, `build`, `test`, `lint`, `format`, `typecheck`; documented in README | — |
| `.env.example` | `.env.example` at root with all required vars; code parses config into typed object at startup (e.g. zod/pydantic-settings) | — |
| Pinned tool versions | Version files (`.nvmrc`, `.tool-versions`, `mise.toml`, or per-lang equivalent); lockfile committed | — |
| Seeding script | `scripts/seed.*`, `db/seeds/`, or `npm run seed` documented | — |

## Observability

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| Structured logging | Logger library configured for JSON output (pino, winston-json, structlog); log level configurable | — |
| Request correlation | Request/trace ID generated or read from header and included in every log line; propagated in outbound calls | — |
| Error tracking | Error-tracker SDK initialized in app entry point | DSN configured in prod |
| Health endpoint | Route handler for `/health`, `/healthz`, `/readyz`, or `/livez` | Monitor calling it |
| Alerting | IaC defines alert rules (Azure `Microsoft.Insights/metricAlerts` or equivalent) | Alert routing / on-call integration |

## Documentation

| Item | In-repo signals | Out-of-repo (→ `Unknown`) |
|---|---|---|
| README: Purpose | README opens with a description of what the project does and who it's for; project status noted | — |
| README: Run | README has "Run" / "Getting started" / "Development" section with commands | — |
| README: Deploy | README or `docs/deploy.md` covers each environment | — |
| README: Architecture | README or `docs/architecture.md` has diagram or component description | — |
| ADRs | `docs/adr/`, `docs/decisions/`, or `adr/` folder with numbered records | — |
| Infrastructure docs | `docs/infrastructure.md` or generated-from-IaC docs describing topology + access | — |

## Testing

| Item | In-repo signals | Out-of-repo (→ `Unknown`) | `N/A` if |
|---|---|---|---|
| Unit testing | Test files (`*.test.*`, `*_test.py`, `spec/`) present; CI runs them | — | — |
| Integration testing | Tests hitting real dependencies (testcontainers, local DB in CI service, in-memory HTTP server against real handlers); CI runs them | — | No service boundaries |
| E2E testing | `e2e/`, `tests/e2e/`, Playwright/Cypress/Selenium config; CI or scheduled workflow runs them | — | Library with no user flows |
