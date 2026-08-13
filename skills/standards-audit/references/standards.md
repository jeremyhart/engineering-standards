# Software Project Standards

The standards a software project is held to, graded across four levels. This file is the index —
what exists and what it's called. Each control is defined per level in [`sections/`](sections/), and
the levels themselves are explained in [`levels.md`](levels.md).

Rate the project with the four questions in `levels.md`, then implement every applicable control at
`max(impact, desired maturity)`.

Standards are numbered within their category — cite them as *Security 4*, *Testing 2*. Categories
are listed alphabetically.

## AI-Assisted Development

Standards for building software with coding agents. See [sections/ai-assisted-development.md](sections/ai-assisted-development.md).

| # | Standard |
|---|---|
| 1 | Agent-readable project context |
| 2 | Agent configuration reviewed as code |
| 3 | Destructive-action guardrails |
| 4 | Automated post-edit checks |
| 5 | Agent issue tracking |
| 6 | Automated diagnosis skills |

## API & Contracts

See [sections/api-and-contracts.md](sections/api-and-contracts.md).

| # | Standard |
|---|---|
| 1 | Interface contract |
| 2 | Published API contract |

## Code Quality

See [sections/code-quality.md](sections/code-quality.md).

| # | Standard |
|---|---|
| 1 | Linting |
| 2 | Formatting |
| 3 | Type safety |

## Continuous Integration

See [sections/continuous-integration.md](sections/continuous-integration.md).

| # | Standard |
|---|---|
| 1 | PR checks required before merge |
| 2 | Dependency update policy |
| 3 | Build provenance |

## Database

*Only relevant if the project has a database.* See [sections/database.md](sections/database.md).

| # | Standard |
|---|---|
| 1 | Database changes only through code |
| 2 | Migrations in pipeline |
| 3 | Migrations rehearsed before production |
| 4 | Non-destructive migrations |
| 5 | Forward-only migrations |

## Deployment

See [sections/deployment.md](sections/deployment.md).

| # | Standard |
|---|---|
| 1 | Automated deployment |
| 2 | Deployment validation |
| 3 | Rollback strategy |
| 4 | Artifact promotion |
| 5 | Feature kill switches |
| 6 | Automated versioning |
| 7 | Automated release notes |

## Developer Environment

See [sections/developer-environment.md](sections/developer-environment.md).

| # | Standard |
|---|---|
| 1 | Setup docs and script |
| 2 | Standard task names |
| 3 | Documented configuration |
| 4 | Validated configuration at startup |
| 5 | Pinned tool versions |
| 6 | Seeding script |

## Development Workflow

The ordering and gating of work. See [sections/development-workflow.md](sections/development-workflow.md).

| # | Standard |
|---|---|
| 1 | Spec-driven development |
| 2 | Test-driven development |
| 3 | Spec-first agent workflow |

## Documentation

See [sections/documentation.md](sections/documentation.md).

| # | Standard |
|---|---|
| 1 | README completeness |
| 2 | Architecture decision records |
| 3 | Infrastructure documentation |
| 4 | Docs drift detection |

## Frontend

*Only relevant if the project has a user-facing UI.* See [sections/frontend.md](sections/frontend.md).

| # | Standard |
|---|---|
| 1 | Accessibility baseline |

## Infrastructure

See [sections/infrastructure.md](sections/infrastructure.md).

| # | Standard |
|---|---|
| 1 | Infrastructure as code |
| 2 | Hosting details documented |
| 3 | Non-production environment |
| 4 | Environment parity |

## Observability

See [sections/observability.md](sections/observability.md).

| # | Standard |
|---|---|
| 1 | Structured logging |
| 2 | Log hygiene |
| 3 | Request correlation |
| 4 | Error tracking |
| 5 | Health endpoint |
| 6 | Alerting |

## Operations

See [sections/operations.md](sections/operations.md).

| # | Standard |
|---|---|
| 1 | Ownership and on-call |
| 2 | Incident response process |
| 3 | Runbooks |
| 4 | Backup and restore |
| 5 | Status communication |
| 6 | Cost monitoring |

## Security

See [sections/security.md](sections/security.md).

| # | Standard |
|---|---|
| 1 | Runtime secrets management |
| 2 | Secret scanning |
| 3 | Dependency vulnerability scanning |
| 4 | Least-privilege access |
| 5 | Production access control |
| 6 | Deny-by-default auth |
| 7 | Endpoint authorization |
| 8 | Secure coding patterns |
| 9 | Audit logging |
| 10 | Personal data handling |
| 11 | Rate limiting and abuse protection |

## Source Control & Review

See [sections/source-control-and-review.md](sections/source-control-and-review.md).

| # | Standard |
|---|---|
| 1 | Branch protection |
| 2 | Code review process |
| 3 | Pre-commit hooks |
| 4 | Commit naming schema |
| 5 | PR template |

## Testing

See [sections/testing.md](sections/testing.md).

| # | Standard |
|---|---|
| 1 | Unit testing |
| 2 | Integration testing |
| 3 | End-to-end testing |
