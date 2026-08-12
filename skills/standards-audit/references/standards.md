# Project Standards Checklist
**Column key**

- **Risk if absent** — *Causes breach* · *Causes incidents* · *Slows team* · *Polish*
- **Min stage** — earliest project stage where this should be in place: *Prototype* · *Internal* · *Prod*
- **Effort** — rough cost to adopt: *S* · *M* · *L*

## AI-Assisted Development

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **CONTEXT.md** — project context file exists and is current (architecture, conventions, gotchas) | Slows team | Prototype | S |
| [ ] | **AGENTS.md** — agent instructions kept in repo alongside CONTEXT.md | Slows team | Prototype | S |
| [ ] | **Single source of truth for agent docs** — one canonical agent instructions file; others (CLAUDE.md, etc.) reference or symlink to it | Slows team | Internal | S |
| [ ] | **Domain model in CONTEXT.md** — ubiquitous language defined; concepts have short shared names used consistently in code, docs & conversation | Slows team | Internal | M |
| [ ] | **ADRs agent-readable** — existing ADRs referenced from CONTEXT.md/AGENTS.md so agents find them (requires ADRs) | Slows team | Prod | S |
| [ ] | **Agent config reviewed as code** — AGENTS.md, skills & hooks live in the repo and go through the same PR review as code | Causes incidents | Internal | S |
| [ ] | **Agent guardrails** — documented boundaries for what agents may/may not do (e.g. no direct prod access) | Causes incidents | Internal | S |
| [ ] | **Destructive-command guardrails as a hook** — hook blocks destructive commands (`rm -rf`, `git push --force`, prod deletes) rather than relying on instructions | Causes incidents | Prod | M |
| [ ] | **Post-tool-use hooks** — lint, format & typecheck run automatically after agent edits | Slows team | Internal | M |
| [ ] | **Issue tracker + triage vocabulary for agents** — agents know where issues live and how to read/file them; minimum is a tracker note in AGENTS.md, scaling to dedicated agent docs (e.g. `docs/agents/issue-tracker.md`) plus a triage label vocabulary (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`) | Slows team | Prod | M |
| [ ] | **Spec-first agent workflow** — non-trivial changes go through an align → spec → tickets → implement → review loop, not one-shot prompts | Causes incidents | Prod | M |
| [ ] | **Automated diagnosis skills** — repo ships skills that know the codebase & environment and diagnose failures automatically (e.g. failed/unhealthy deployments) | Causes incidents | Prod | L |

## Source Control

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Branch protection** — main branch requires PR; no direct pushes or force pushes | Causes incidents | Internal | S |
| [ ] | **Code review process** — minimum reviewers defined; CODEOWNERS file; review expectations documented | Causes incidents | Prod | S |
| [ ] | **Pre-commit hooks** — lint, format & typecheck run locally before commit | Slows team | Internal | S |
| [ ] | **Commit naming schema** — Conventional Commits enforced (e.g. commitlint) | Polish | Prod | S |
| [ ] | **PR template** — `.github/PULL_REQUEST_TEMPLATE.md` with summary, testing & checklist sections | Slows team | Internal | S |

## CI

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **PR checks required before merge** — build, lint, typecheck & tests must pass to merge | Causes incidents | Internal | S |
| [ ] | **Secret scanning** — scanner runs in pipeline and blocks on findings | Causes breach | Internal | S |
| [ ] | **Dependency vuln scanning** — automated dependency update bot enabled + audit step in CI | Causes incidents | Internal | S |
| [ ] | **Dependency update policy** — cadence for merging dependency-update PRs; major-version strategy | Slows team | Prod | S |
| [ ] | **Docs drift detection** — drift between docs and code detected automatically (CI gate and/or agent skill) and drives updates | Slows team | Prod | L |

## Deployment

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Continuous deployment** — deploys automated from main via pipeline; no manual deploys | Slows team | Prod | M |
| [ ] | **Deployment validation** — post-deploy smoke test/health check gates the release | Causes incidents | Prod | M |
| [ ] | **Rollback strategy** — documented, tested procedure to redeploy the previous version | Causes incidents | Prod | M |
| [ ] | **Automated rollback on failed smoke test** — pipeline rolls back to previous version automatically when post-deploy validation fails | Causes incidents | Prod | L |
| [ ] | **Guarded release/deploy wrappers** — deploys go through wrapper scripts that enforce checks; no raw deploy commands | Causes incidents | Prod | M |
| [ ] | **Promotion path defined by trigger** — what deploys where is defined by pipeline triggers (branch/tag), not by human choice | Causes incidents | Prod | M |
| [ ] | **Artifact promotion** — same build artifact promoted staging → prod, not rebuilt per environment | Causes incidents | Prod | M |
| [ ] | **Feature kill switches** — risky features can be disabled via config without a deploy; minimum is env-var flags, scaling to a flag service with gradual rollout | Causes incidents | Prod | S |
| [ ] | **Automated versioning** — version bumped automatically from commit history | Polish | Prod | M |
| [ ] | **Automated release notes** — changelog/release notes generated on release | Polish | Prod | S |

## Infrastructure

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **IaC** — all infrastructure defined in code; no click-ops resources | Causes incidents | Internal | M |
| [ ] | **Hosting details documented** — accounts, resource names, regions & identifiers recorded | Slows team | Internal | S |
| [ ] | **Environment parity** — dev/staging/prod environments defined with documented differences | Causes incidents | Prod | M |
| [ ] | **Backup & restore procedure** — backups automated; restore procedure documented and tested | Causes incidents | Prod | M |

## Security

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Runtime secrets management** — secrets in a managed secret store/workload identity; none in config or repo | Causes breach | Internal | M |
| [ ] | **Least-privilege access** — cloud resource access scoped to minimum required; roles documented | Causes incidents | Prod | M |
| [ ] | **Prod access control** — who has production access is defined and reviewed | Causes breach | Prod | S |
| [ ] | **Auth gates deny by default** — every endpoint requires auth unless explicitly marked public; enforced centrally (middleware/gateway), fails closed | Causes breach | Internal | M |
| [ ] | **Authorization reviewed per endpoint** — each endpoint's authz rules explicit and reviewed; no ad-hoc per-route logic | Causes breach | Prod | M |
| [ ] | **Secure coding baseline** — input validated at boundaries, output encoded, parameterised queries (OWASP basics) | Causes breach | Internal | M |
| [ ] | **Audit logging** — security-relevant events (logins, permission changes, data exports) logged immutably | Causes breach | Prod | M |
| [ ] | **Personal data handling** — PII identified and documented; deletion path implemented; retention enforced in code/schema (engineering artifacts only, not the legal/compliance layer) | Causes breach | Prod | M |
| [ ] | **Rate limiting & abuse protection** — public and costly endpoints throttled; minimum is platform-level rate limiting (gateway/WAF), scaling to per-user quotas and anomaly alerts | Causes incidents | Prod | M |

## Operations

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Ownership & on-call** — service owner defined; who responds to incidents is clear | Causes incidents | Prod | S |
| [ ] | **Incident response process** — defined path from alert → owner → fix → postmortem; minimum is a written "who to ping, where to triage" note, scaling to on-call rotation and blameless postmortems | Causes incidents | Prod | S |
| [ ] | **Runbooks** — documented procedures for known failure modes | Slows team | Prod | M |
| [ ] | **Status communication** — users can find out if the service is degraded without contacting you; minimum is a pinned notice channel, scaling to an automated status page (external users only) | Polish | Prod | S |
| [ ] | **Cost monitoring** — budget alerts configured; spend reviewed regularly | Causes incidents | Internal | S |

## Code Quality

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Linting** — linter configured, zero errors, enforced in CI | Slows team | Prototype | S |
| [ ] | **Formatting** — formatter configured, enforced in CI | Slows team | Prototype | S |
| [ ] | **Typecheck** — strictest type checking enabled, zero errors, enforced in CI | Causes incidents | Internal | M |
| [ ] | **API contract defined** — API surface described by a machine-readable contract (OpenAPI/tRPC/GraphQL schema); minimum is shared types between client & server, scaling to breaking-change detection in CI | Causes incidents | Internal | M |

## Frontend

*Only relevant if the project has a user-facing UI.*

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Accessibility baseline** — automated a11y checks (axe/Lighthouse) in CI; minimum is zero critical violations, scaling to manual keyboard/screen-reader passes | Causes incidents | Prod | M |

## Database

*Only relevant if the project has a database.*

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Migrations** — all schema changes via versioned migrations; no manual schema edits | Causes incidents | Internal | M |
| [ ] | **Migrations in pipeline** — migrations applied automatically in the deploy pipeline, before code deploy | Causes incidents | Prod | M |
| [ ] | **Migrations rehearsed on staging** — every migration runs on staging before production | Causes incidents | Prod | S |
| [ ] | **Non-destructive migrations** — migrations never drop/rename in a way that breaks the running version (expand–contract pattern) | Causes incidents | Prod | M |
| [ ] | **Forward-only migrations** — no down migrations; rollback restores the previous app version, not the schema | Slows team | Prod | S |

## Developer Environment

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Setup docs & script** — new dev can go from clone to running app with one documented script | Slows team | Prototype | S |
| [ ] | **Standard task names** — standard task-runner entry points (`dev`, `build`, `test`, `lint`, `format`, `typecheck`) documented | Slows team | Prototype | S |
| [ ] | **`.env.example`** — all required variables listed; config parsed into a typed object at startup with clear errors | Causes incidents | Prototype | S |
| [ ] | **Pinned tool versions** — runtime & toolchain versions pinned; lockfile committed | Causes incidents | Internal | S |
| [ ] | **Seeding script** — repeatable script populates local DB with representative data | Slows team | Internal | M |

## Observability

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Structured logging** — consistent, structured (JSON) logs with levels | Causes incidents | Prod | S |
| [ ] | **Request correlation** — request/trace ID attached to every log line; propagated across service boundaries where they exist (full distributed tracing at higher maturity) | Slows team | Internal | S |
| [ ] | **Error tracking** — unhandled errors captured and reported to an error tracker | Causes incidents | Prod | S |
| [ ] | **Health endpoint** — liveness/readiness endpoint exposed and monitored | Causes incidents | Prod | S |
| [ ] | **Alerting** — alerts configured for failures and key thresholds | Causes incidents | Prod | M |

## Documentation

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **README: Purpose** — what the project does, who it's for, and its status in the first screenful | Slows team | Prototype | S |
| [ ] | **README: Run** — how to run locally | Slows team | Prototype | S |
| [ ] | **README: Deploy** — how to deploy each environment | Slows team | Internal | S |
| [ ] | **README: Architecture** — high-level diagram/description of system components | Slows team | Internal | M |
| [ ] | **ADRs** — architecture decision records kept for significant decisions | Slows team | Prod | M |
| [ ] | **Infrastructure docs** — resources, topology & access documented (or generated from IaC) | Slows team | Prod | M |

## Testing

| Done | Item | Risk if absent | Min stage | Effort |
|---|---|---|---|---|
| [ ] | **Unit testing** — core logic covered; runs in CI | Causes incidents | Internal | M |
| [ ] | **Integration testing** — service boundaries (DB queries, HTTP handlers) tested against real dependencies | Causes incidents | Prod | M |
| [ ] | **E2E testing** — critical user flows covered; runs in CI or on schedule | Causes incidents | Prod | L |
