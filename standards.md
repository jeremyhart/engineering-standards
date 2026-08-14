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
| 1 | [Agent-readable project context](sections/ai-assisted-development.md#agent-readable-project-context) | The project explains itself to an agent without a human in the loop. |
| 2 | [Agent configuration reviewed as code](sections/ai-assisted-development.md#agent-configuration-reviewed-as-code) | Agent instructions, skills and hooks go through the same review as application code. |
| 3 | [Destructive-action guardrails](sections/ai-assisted-development.md#destructive-action-guardrails) | Boundaries on what agents may do, enforced by machinery rather than instructions. |
| 4 | [Automated post-edit checks](sections/ai-assisted-development.md#automated-post-edit-checks) | Lint and format run automatically after an agent edits code. |
| 5 | [Agent issue tracking](sections/ai-assisted-development.md#agent-issue-tracking) | Agents know where work is tracked and how to read and file it. |
| 6 | [Automated diagnosis skills](sections/ai-assisted-development.md#automated-diagnosis-skills) | The repo ships skills that can diagnose failures without a human reconstructing context first. |

## API & Contracts

The dividing line: can you deploy both sides at once? If you can, the contract catches drift; if you
can't, it's a promise that costs a version and a conversation to change. See
[sections/api-and-contracts.md](sections/api-and-contracts.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Interface contract](sections/api-and-contracts.md#interface-contract) | The API shape is described somewhere both sides derive from, so client and server can't drift apart. |
| 2 | [Published API contract](sections/api-and-contracts.md#published-api-contract) | The API is a promise to outside consumers, so changes are versioned, announced and supported. |

## Code Quality

Static checks on the code itself, enforced at the same three points as everything else: agent
post-edit hook, pre-commit, and CI as the authority. See
[sections/code-quality.md](sections/code-quality.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Linting](sections/code-quality.md#linting) | A linter is configured, its findings are acted on, and it's enforced rather than advisory. |
| 2 | [Formatting](sections/code-quality.md#formatting) | Formatting is automated and not a matter of opinion or review comments. |
| 3 | [Type safety](sections/code-quality.md#type-safety) | Types are checked automatically, and strictness rises with the cost of being wrong. |

## Continuous Integration

CI owns the gate and the build. See
[sections/continuous-integration.md](sections/continuous-integration.md).

| # | Standard | Description |
|---|---|---|
| 1 | [PR checks required before merge](sections/continuous-integration.md#pr-checks-required-before-merge) | Build, checks and tests run on every change, and failing them blocks the merge. |
| 2 | [Dependency update policy](sections/continuous-integration.md#dependency-update-policy) | There's a stated cadence for taking dependency updates. |
| 3 | [Build provenance](sections/continuous-integration.md#build-provenance) | You can say what a shipped artifact was built from and prove it wasn't altered afterwards. |

## Database

*Only relevant if the project has a database.* The unifying idea: nobody changes the data model by
hand — everything arrives through code, reviewed and repeatable. See
[sections/database.md](sections/database.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Database changes only through code](sections/database.md#database-changes-only-through-code) | Schema and data changes ship as versioned migrations in the repository, reviewed like any other code. |
| 2 | [Migrations in pipeline](sections/database.md#migrations-in-pipeline) | Migrations are applied automatically by the deploy pipeline, before the code that depends on them. |
| 3 | [Migrations rehearsed before production](sections/database.md#migrations-rehearsed-before-production) | Every migration runs somewhere else first, against realistic data, before it touches production. |
| 4 | [Non-destructive migrations](sections/database.md#non-destructive-migrations) | Migrations never drop or rename in a way that breaks the currently running version. |
| 5 | [Forward-only migrations](sections/database.md#forward-only-migrations) | There are no down migrations — recovery is redeploying the previous version, not reversing the schema. |

## Deployment

See [sections/deployment.md](sections/deployment.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Automated deployment](sections/deployment.md#automated-deployment) | Code reaches production through a pipeline; nobody hand-copies files or picks a target. |
| 2 | [Deployment validation](sections/deployment.md#deployment-validation) | A deploy isn't finished until something has checked that it's actually working. |
| 3 | [Rollback strategy](sections/deployment.md#rollback-strategy) | There is a known, practised way back to the previous version. |
| 4 | [Artifact promotion](sections/deployment.md#artifact-promotion) | The thing you tested is the thing you ship — one artifact moves through environments. |
| 5 | [Feature kill switches](sections/deployment.md#feature-kill-switches) | Risky behaviour can be turned off without a deploy. |
| 6 | [Automated versioning](sections/deployment.md#automated-versioning) | Version numbers come from commit history rather than someone remembering to bump a file. |
| 7 | [Automated release notes](sections/deployment.md#automated-release-notes) | What changed in a release is generated, not written from memory. |

## Developer Environment

See [sections/developer-environment.md](sections/developer-environment.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Setup docs and script](sections/developer-environment.md#setup-docs-and-script) | Someone with a clean machine can go from clone to running application by following one documented path. |
| 2 | [Standard task names](sections/developer-environment.md#standard-task-names) | Common operations have predictable entry points, the same names as everywhere else. |
| 3 | [Documented configuration](sections/developer-environment.md#documented-configuration) | Every configuration value is listed, with an example, before anyone has to discover it from a crash. |
| 4 | [Validated configuration at startup](sections/developer-environment.md#validated-configuration-at-startup) | Missing or malformed configuration fails immediately and says what's wrong. |
| 5 | [Pinned tool versions](sections/developer-environment.md#pinned-tool-versions) | The toolchain is fixed, so builds don't change because someone upgraded something. |
| 6 | [Seeding script](sections/developer-environment.md#seeding-script) | A repeatable script fills a local database with representative data. |

## Development Workflow

The ordering and gating of work: intent stated before implementation, at three altitudes. See
[sections/development-workflow.md](sections/development-workflow.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Spec-driven development](sections/development-workflow.md#spec-driven-development) | What a change should do is agreed and written down before it is built. |
| 2 | [Test-driven development](sections/development-workflow.md#test-driven-development) | Behaviour is expressed as a test before it is implemented. |
| 3 | [Spec-first agent workflow](sections/development-workflow.md#spec-first-agent-workflow) | Non-trivial agent work goes through align → spec → tasks → implement → review. |

## Documentation

See [sections/documentation.md](sections/documentation.md).

| # | Standard | Description |
|---|---|---|
| 1 | [README completeness](sections/documentation.md#readme-completeness) | The README answers, in order: what this is, how to run it, how to deploy it, how it's put together. |
| 2 | [Architecture decision records](sections/documentation.md#architecture-decision-records) | Significant decisions are recorded with their context and consequences. |
| 3 | [Infrastructure documentation](sections/documentation.md#infrastructure-documentation) | What exists, where it lives, and who can reach it. |
| 4 | [Docs drift detection](sections/documentation.md#docs-drift-detection) | Documentation that has drifted from the code is found automatically. |

## Frontend

*Only relevant if the project has a user-facing UI.* See
[sections/frontend.md](sections/frontend.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Accessibility baseline](sections/frontend.md#accessibility-baseline) | The interface is usable by people who don't use it the way you do. |

## Infrastructure

See [sections/infrastructure.md](sections/infrastructure.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Infrastructure as code](sections/infrastructure.md#infrastructure-as-code) | Infrastructure is defined in the repository and applied by tooling, not clicked into being. |
| 2 | [Hosting details documented](sections/infrastructure.md#hosting-details-documented) | Where the thing actually runs is written down. |
| 3 | [Non-production environment](sections/infrastructure.md#non-production-environment) | There is somewhere other than production to try things. |
| 4 | [Environment parity](sections/infrastructure.md#environment-parity) | Environments differ only in ways you know about and have written down. |

## Observability

Seeing inside the running system. Responding to what you see belongs to Operations. See
[sections/observability.md](sections/observability.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Structured logging](sections/observability.md#structured-logging) | Logs are machine-readable and consistent, so they can be searched rather than read. |
| 2 | [Log hygiene](sections/observability.md#log-hygiene) | Logs don't become the place your personal data quietly lives forever. |
| 3 | [Request correlation](sections/observability.md#request-correlation) | A single request can be followed across everything it touched. |
| 4 | [Error tracking](sections/observability.md#error-tracking) | Unhandled errors reach somewhere a human will see them. |
| 5 | [Health endpoint](sections/observability.md#health-endpoint) | The system can be asked whether it is working. |
| 6 | [Alerting](sections/observability.md#alerting) | Something tells a human when the system needs attention, and it's worth waking for. |

## Operations

Running the system and responding when it misbehaves. See
[sections/operations.md](sections/operations.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Ownership and on-call](sections/operations.md#ownership-and-on-call) | It is clear who is responsible, and who picks up the phone. |
| 2 | [Incident response process](sections/operations.md#incident-response-process) | There is a known path from "something is wrong" to "it's fixed and we learned something". |
| 3 | [Runbooks](sections/operations.md#runbooks) | The known failure modes have written responses, so recovery doesn't depend on who is awake. |
| 4 | [Backup and restore](sections/operations.md#backup-and-restore) | The data can be brought back, and you know that because you've done it. |
| 5 | [Status communication](sections/operations.md#status-communication) | Users can find out that the service is degraded without asking you. |
| 6 | [Cost monitoring](sections/operations.md#cost-monitoring) | Spend is visible before it becomes a surprise. |

## Security

Filed by concern, not by mechanism — secret and dependency scanning live here even though they run
in the pipeline. See [sections/security.md](sections/security.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Runtime secrets management](sections/security.md#runtime-secrets-management) | Secrets live in a managed store, not in the repository and not in configuration files. |
| 2 | [Secret scanning](sections/security.md#secret-scanning) | Committed secrets are found automatically, with a way to handle findings that doesn't end in the scanner being switched off. |
| 3 | [Dependency vulnerability scanning](sections/security.md#dependency-vulnerability-scanning) | You know when something you depend on becomes dangerous, or its licence becomes a problem. |
| 4 | [Least-privilege access](sections/security.md#least-privilege-access) | Every identity has the narrowest access that lets it work. |
| 5 | [Production access control](sections/security.md#production-access-control) | Who can reach production is deliberate, known and revocable. |
| 6 | [Deny-by-default auth](sections/security.md#deny-by-default-auth) | Authentication is defined once and applied everywhere; anything not explicitly public is protected. |
| 7 | [Endpoint authorization](sections/security.md#endpoint-authorization) | Being logged in is not the same as being allowed, and the difference is explicit. |
| 8 | [Secure coding patterns](sections/security.md#secure-coding-patterns) | The code uses patterns that make whole classes of vulnerability impossible, rather than relying on care. |
| 9 | [Audit logging](sections/security.md#audit-logging) | Security-relevant events are recorded in a way that survives the person who caused them. |
| 10 | [Personal data handling](sections/security.md#personal-data-handling) | You know what personal data you hold, where it is, and how to remove it. |
| 11 | [Rate limiting and abuse protection](sections/security.md#rate-limiting-and-abuse-protection) | Public and expensive endpoints can't be used to exhaust you. |

## Source Control & Review

Version control and the collaboration process around it. See
[sections/source-control-and-review.md](sections/source-control-and-review.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Branch protection](sections/source-control-and-review.md#branch-protection) | The main branch can't be changed by accident. |
| 2 | [Code review process](sections/source-control-and-review.md#code-review-process) | Changes are looked at by someone other than their author, where someone else exists. |
| 3 | [Pre-commit hooks](sections/source-control-and-review.md#pre-commit-hooks) | Fast checks run before a commit is made, so obvious problems never enter history. |
| 4 | [Commit naming schema](sections/source-control-and-review.md#commit-naming-schema) | Commit messages follow a convention, so history is readable and releases can be derived from it. |
| 5 | [PR template](sections/source-control-and-review.md#pr-template) | Pull requests arrive with the context a reviewer needs. |

## Testing

The test suite as an asset: what exists and what it covers. *When* tests are written belongs to
Development Workflow. See [sections/testing.md](sections/testing.md).

| # | Standard | Description |
|---|---|---|
| 1 | [Unit testing](sections/testing.md#unit-testing) | The logic that matters is covered by fast tests that run on every change. |
| 2 | [Integration testing](sections/testing.md#integration-testing) | The seams are tested against real dependencies, not mocks of your own assumptions. |
| 3 | [End-to-end testing](sections/testing.md#end-to-end-testing) | The journeys that matter are exercised the way a user would. |
