# Software Project Standards

The standards a software project is held to, graded across four levels. This file is the index —
what exists, what it's called, and what it means in one line. Each control is defined in full, per
level, in [`sections/`](sections/) — click a standard's name to jump straight to its definition. The
levels themselves are explained in [`levels.md`](levels.md).

Rate the project with the four questions in `levels.md`, then implement every applicable control at
`max(impact, desired maturity)`.

Standards are numbered within their category — cite them as *Security 4*, *Testing 2*. Categories
are listed alphabetically.

## AI-Assisted Development

Standards for building software with coding agents — not about features that call an LLM at runtime.
See [sections/ai-assisted-development.md](sections/ai-assisted-development.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Agent-readable project context](sections/ai-assisted-development.md#agent-readable-project-context) | A committed file (e.g. `AGENTS.md` or `CONTEXT.md`) documents architecture, conventions and gotchas before an agent touches code. |
| 2 | [Agent configuration reviewed as code](sections/ai-assisted-development.md#agent-configuration-reviewed-as-code) | Agent instructions, skills and hooks are committed to the repo and go through the same PR review as application code. |
| 3 | [Destructive-action guardrails](sections/ai-assisted-development.md#destructive-action-guardrails) | Actions like force pushes, production access and recursive deletes are blocked by a hook or wrapper, not left to instructions an agent can ignore. |
| 4 | [Automated post-edit checks](sections/ai-assisted-development.md#automated-post-edit-checks) | A formatter and linter run automatically after every agent edit and auto-fix what they can. |
| 5 | [Agent issue tracking](sections/ai-assisted-development.md#agent-issue-tracking) | The issue tracker and its labels are named in agent context, with a triage vocabulary marking what's ready for an agent. |
| 6 | [Automated diagnosis skills](sections/ai-assisted-development.md#automated-diagnosis-skills) | A skill queries deploy state and logs for the project's known failure modes and reports a cause, instead of a human reconstructing context from a log dump. |

## API & Contracts

The dividing line: can you deploy both sides at once? If you can, the contract catches drift; if you
can't, it's a promise that costs a version and a conversation to change. See
[sections/api-and-contracts.md](sections/api-and-contracts.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Interface contract](sections/api-and-contracts.md#interface-contract) | Client and server share generated types or a schema-derived client instead of hand-written duplicates kept in sync by hand. |
| 2 | [Published API contract](sections/api-and-contracts.md#published-api-contract) | Breaking changes ship as a new, announced API version with a deprecation window, rather than mutating what existing consumers already call. |

## Code Quality

Static checks on the code itself, enforced at the same three points as everything else: agent
post-edit hook, pre-commit, and CI as the authority. See
[sections/code-quality.md](sections/code-quality.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Linting](sections/code-quality.md#linting) | A linter runs in CI with zero errors allowed, and its rules cover correctness and security patterns, not just style. |
| 2 | [Formatting](sections/code-quality.md#formatting) | A formatter with committed settings runs across the whole codebase in CI, so no diff carries unrelated reformatting. |
| 3 | [Type safety](sections/code-quality.md#type-safety) | A type checker runs in CI with zero errors, and escape hatches like `any` or ignore comments need a justifying comment. |

## Continuous Integration

CI owns the gate and the build. See
[sections/continuous-integration.md](sections/continuous-integration.md).

| # | Standard | Description |
|---|---|---|
| 1 | [PR checks required before merge](sections/continuous-integration.md#pr-checks-required-before-merge) | Build, lint, types and tests are required status checks that block the merge button, not just a red badge someone can ignore. |
| 2 | [Dependency update policy](sections/continuous-integration.md#dependency-update-policy) | An update bot's PRs are reviewed and merged on a stated cadence instead of left open indefinitely. |
| 3 | [Build provenance](sections/continuous-integration.md#build-provenance) | The artifact records the commit it was built from, and at the top level ships with a signed SBOM. |

## Database

*Only relevant if the project has a database.* The unifying idea: nobody changes the data model by
hand — everything arrives through code, reviewed and repeatable. See
[sections/database.md](sections/database.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Database changes only through code](sections/database.md#database-changes-only-through-code) | Schema and data changes are versioned migration files reviewed in a PR, never commands typed into a console. |
| 2 | [Migrations in pipeline](sections/database.md#migrations-in-pipeline) | The deploy pipeline runs migrations automatically, before deploying the code that depends on them. |
| 3 | [Migrations rehearsed before production](sections/database.md#migrations-rehearsed-before-production) | Every migration runs against a non-production environment with realistic data before it touches production. |
| 4 | [Non-destructive migrations](sections/database.md#non-destructive-migrations) | Migrations expand and backfill before they contract, so the currently running version is never broken by a schema change. |
| 5 | [Forward-only migrations](sections/database.md#forward-only-migrations) | There are no down migrations; recovery means redeploying the previous app version against the current schema. |

## Deployment

See [sections/deployment.md](sections/deployment.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Automated deployment](sections/deployment.md#automated-deployment) | A pipeline deploys on merge or tag; nobody runs a deploy script by hand or picks a target. |
| 2 | [Deployment validation](sections/deployment.md#deployment-validation) | A post-deploy check exercises a real user path and treats its failure as a failed deploy, not just a health-endpoint ping. |
| 3 | [Rollback strategy](sections/deployment.md#rollback-strategy) | Redeploying the previous version is a rehearsed, timed procedure, not something worked out from memory during an incident. |
| 4 | [Artifact promotion](sections/deployment.md#artifact-promotion) | The same build artifact moves unchanged from staging to production, rather than being rebuilt per environment. |
| 5 | [Feature kill switches](sections/deployment.md#feature-kill-switches) | Risky behaviour sits behind a flag that can be flipped off without a deploy. |
| 6 | [Automated versioning](sections/deployment.md#automated-versioning) | The version number is derived from commit history at release time, not bumped by hand in a file. |
| 7 | [Automated release notes](sections/deployment.md#automated-release-notes) | A changelog is generated from commit history on release rather than written from memory. |

## Developer Environment

See [sections/developer-environment.md](sections/developer-environment.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Setup docs and script](sections/developer-environment.md#setup-docs-and-script) | One documented command takes a clean checkout to a running app, with no undocumented local state or "ask someone for the file". |
| 2 | [Standard task names](sections/developer-environment.md#standard-task-names) | `dev`, `build`, `test`, `lint`, `format` and `typecheck` are the entry points, and CI calls the same ones rather than its own commands. |
| 3 | [Documented configuration](sections/developer-environment.md#documented-configuration) | Every config variable is listed in an example file with what it does, a valid value, and whether it's optional. |
| 4 | [Validated configuration at startup](sections/developer-environment.md#validated-configuration-at-startup) | The app parses and validates config once at startup and fails immediately naming what's missing, instead of surfacing a crash later. |
| 5 | [Pinned tool versions](sections/developer-environment.md#pinned-tool-versions) | Runtime and toolchain versions are pinned in a file CI reads too, so an upgrade on one machine can't silently change a build. |
| 6 | [Seeding script](sections/developer-environment.md#seeding-script) | A script populates a local database with production-shaped data, containing no copied production personal data. |

## Development Workflow

The ordering and gating of work: intent stated before implementation, at three altitudes. See
[sections/development-workflow.md](sections/development-workflow.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Spec-driven development](sections/development-workflow.md#spec-driven-development) | Acceptance criteria are written down and reviewed before implementation starts, not backfilled to match what got built. |
| 2 | [Test-driven development](sections/development-workflow.md#test-driven-development) | Every behaviour change ships with a test change in the same diff, enforced by a coverage gate on the changed lines. |
| 3 | [Spec-first agent workflow](sections/development-workflow.md#spec-first-agent-workflow) | Non-trivial agent work follows align → spec → tasks → implement → review with a human checkpoint before implementation, not a single prompt. |

## Documentation

See [sections/documentation.md](sections/documentation.md).

| # | Standard | Description |
|---|---|---|
| 1 | [README completeness](sections/documentation.md#readme-completeness) | The README covers purpose, local run, deploy, and architecture, in that order. |
| 2 | [Architecture decision records](sections/documentation.md#architecture-decision-records) | Significant decisions are recorded in a consistent, dated format, naming the rejected alternatives and why. |
| 3 | [Infrastructure documentation](sections/documentation.md#infrastructure-documentation) | Resources, regions and access are listed somewhere other than one person's memory, generated from infrastructure code where possible. |
| 4 | [Docs drift detection](sections/documentation.md#docs-drift-detection) | A scheduled check compares docs against the code and reports what's out of date, instead of waiting for someone to notice. |

## Frontend

*Only relevant if the project has a user-facing UI.* See
[sections/frontend.md](sections/frontend.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Accessibility baseline](sections/frontend.md#accessibility-baseline) | Semantic markup, labelled controls and keyboard navigation, checked automatically in CI with zero critical violations. |

## Infrastructure

See [sections/infrastructure.md](sections/infrastructure.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Infrastructure as code](sections/infrastructure.md#infrastructure-as-code) | Resources are defined in code with state stored centrally and locked; nobody creates production resources by clicking in a console. |
| 2 | [Hosting details documented](sections/infrastructure.md#hosting-details-documented) | Account IDs, regions, DNS records, the pipeline, and third-party dependencies are written down and kept current. |
| 3 | [Non-production environment](sections/infrastructure.md#non-production-environment) | A staging environment mirrors production's services and deploy path, not just "local dev plus production". |
| 4 | [Environment parity](sections/infrastructure.md#environment-parity) | Environments are provisioned from the same infrastructure code with parameters, and undocumented differences are treated as defects. |

## Observability

Seeing inside the running system. Responding to what you see belongs to Operations. See
[sections/observability.md](sections/observability.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Structured logging](sections/observability.md#structured-logging) | Logs are JSON (or equivalent) with consistent fields and severities, shipped off-host and kept long enough to investigate. |
| 2 | [Log hygiene](sections/observability.md#log-hygiene) | A field deny-list redacts credentials and personal data at the logger, under a retention window separate from audit logs. |
| 3 | [Request correlation](sections/observability.md#request-correlation) | A request or trace ID is on every log line and propagates across service boundaries and into background jobs. |
| 4 | [Error tracking](sections/observability.md#error-tracking) | Unhandled errors are captured with stack trace and context in an error tracker, grouped and triaged rather than piling up. |
| 5 | [Health endpoint](sections/observability.md#health-endpoint) | An endpoint distinguishes liveness from readiness, checks real dependencies, and is polled continuously from outside. |
| 6 | [Alerting](sections/observability.md#alerting) | Every alert names an owner and links a runbook; alerts nobody acts on are retuned or removed. |

## Operations

Running the system and responding when it misbehaves. See
[sections/operations.md](sections/operations.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Ownership and on-call](sections/operations.md#ownership-and-on-call) | A named owner and on-call rotation are written down, with an escalation path for when they don't answer. |
| 2 | [Incident response process](sections/operations.md#incident-response-process) | A documented path runs from alert to owner to fix to review, with severity levels and blameless postmortems for the significant ones. |
| 3 | [Runbooks](sections/operations.md#runbooks) | Each alert links a runbook accurate enough to follow at 3am without prior context, rehearsed and revised on a schedule. |
| 4 | [Backup and restore](sections/operations.md#backup-and-restore) | Backups are automated and monitored, and the restore has actually been performed with the time it took recorded. |
| 5 | [Status communication](sections/operations.md#status-communication) | A status page tells users about degradation without them contacting anyone, updated within a stated timescale. |
| 6 | [Cost monitoring](sections/operations.md#cost-monitoring) | Budget alerts catch anomalies, not just totals, and spend is attributed per service or tenant. |

## Security

Filed by concern, not by mechanism — secret and dependency scanning live here even though they run
in the pipeline. See [sections/security.md](sections/security.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Runtime secrets management](sections/security.md#runtime-secrets-management) | Secrets come from a managed store with workload identity, not environment files or committed config, and rotate on a schedule. |
| 2 | [Secret scanning](sections/security.md#secret-scanning) | A scanner runs on every change and blocks merge on findings, with a baseline for false positives and an expiry date on each suppression. |
| 3 | [Dependency vulnerability scanning](sections/security.md#dependency-vulnerability-scanning) | A scanner blocks merge on high/critical findings and checks licences against an allowlist; an update bot raises the fixes. |
| 4 | [Least-privilege access](sections/security.md#least-privilege-access) | Each person and service has its own identity scoped to what it needs, reviewed on a schedule rather than granted once. |
| 5 | [Production access control](sections/security.md#production-access-control) | Who can reach production is a short, known list granted explicitly, moving toward time-boxed break-glass access only. |
| 6 | [Deny-by-default auth](sections/security.md#deny-by-default-auth) | Authentication is enforced once, centrally, for every route; a broken check denies by default instead of allowing through. |
| 7 | [Endpoint authorization](sections/security.md#endpoint-authorization) | Endpoints check the resource belongs to the caller, with tenant isolation proven by an automated test on every change. |
| 8 | [Secure coding patterns](sections/security.md#secure-coding-patterns) | Queries are parameterised, input is validated against a schema at the boundary, and output is encoded by default, enforced by static analysis in CI. |
| 9 | [Audit logging](sections/security.md#audit-logging) | Who did what and when is recorded in a log the application itself can't edit, kept separate from and outliving application logs. |
| 10 | [Personal data handling](sections/security.md#personal-data-handling) | What personal data exists and where is documented, with a tested deletion path that accounts for logs and backups. |
| 11 | [Rate limiting and abuse protection](sections/security.md#rate-limiting-and-abuse-protection) | Gateway-level rate limits plus per-user quotas on costly operations, with anomalies raising an alert. |

## Source Control & Review

Version control and the collaboration process around it. See
[sections/source-control-and-review.md](sections/source-control-and-review.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Version control](sections/source-control-and-review.md#version-control) | Code is managed in a version control system. |
| 2 | [Branch protection](sections/source-control-and-review.md#branch-protection) | Main blocks direct and force pushes and requires passing checks before merge, with no exception for administrators. |
| 3 | [Code review process](sections/source-control-and-review.md#code-review-process) | At least one approving review from someone other than the author is required to merge, routed by CODEOWNERS. |
| 4 | [Pre-commit hooks](sections/source-control-and-review.md#pre-commit-hooks) | Formatting and fast lint run on staged files before commit, installed automatically as part of setup so nobody opts out. |
| 5 | [Commit naming schema](sections/source-control-and-review.md#commit-naming-schema) | Commit messages follow a convention enforced by tooling, which is what makes automated versioning and changelogs possible. |
| 6 | [PR template](sections/source-control-and-review.md#pr-template) | A template prompts for what changed, why, how it was tested, and project-specific checks like migrations or rollback. |

## Testing

The test suite as an asset: what exists and what it covers. *When* tests are written belongs to
Development Workflow. See [sections/testing.md](sections/testing.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Unit testing](sections/testing.md#unit-testing) | Fast tests cover the logic that carries risk, run in CI, block the merge on failure, and isolate their own data. |
| 2 | [Integration testing](sections/testing.md#integration-testing) | Database queries and HTTP handlers run in CI against real ephemeral instances, not mocks of your own assumptions. |
| 3 | [End-to-end testing](sections/testing.md#end-to-end-testing) | The critical user journeys are exercised the way a user would, in CI or on a schedule, and again against production post-deploy. |
