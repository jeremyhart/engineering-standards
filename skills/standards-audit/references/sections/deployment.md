# Deployment

## Automated deployment

Code reaches production through a pipeline. Nobody copies files onto a server, and nobody chooses
by hand where a build lands.

That is the whole point of this control: **no human hand-copying code into production**. The levels
are the road to CI/CD.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Deploying is one documented, repeatable command or script — not dragging files, not a sequence remembered from last time | S |
| 2 · Shared | The pipeline deploys on merge, running the checks first; the wrapper script's checks move into the pipeline | M |
| 3 · Critical | What deploys where is determined by the trigger (branch or tag), not by a person selecting a target. No local deploys are possible | M |
| 4 · Contracted | Every deployment is recorded — what shipped, from which commit, when, triggered by what — and that record is retained | M |

## Deployment validation

A deploy isn't finished until something has checked that the thing that started is actually working.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | A smoke check runs after deploy — at minimum, the app responds and its health endpoint is green | S |
| 3 · Critical | Post-deploy validation gates the release: it covers a real user path, and a failure is treated as a failed deploy | M |
| 4 · Contracted | Validation runs against each environment in the promotion path, and failure blocks promotion automatically | M |

## Rollback strategy

There is a known, practised way back to the previous version.

**Requires:** *Non-destructive migrations* (Database 4) at level 3 and above. Automated rollback over
destructive migrations restores old code against a new schema — that is worse than no automation.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | The procedure to redeploy the previous version is written down and someone has done it at least once | S |
| 3 · Critical | Rollback is tested, not just documented — rehearsed on a schedule, and fast enough to be the first response to a bad deploy | M |
| 4 · Contracted | The pipeline rolls back automatically when post-deploy validation fails, and alerts | L |

## Artifact promotion

The thing you tested is the thing you ship: one build artifact moves through environments rather
than being rebuilt for each.

Some stacks bake configuration at build time, which makes a single promotable artifact impossible
without moving configuration to runtime. The facet files name where this bites.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Configuration is externalised from the build, even if each environment still builds separately | M |
| 3 · Critical | The same artifact is promoted from the non-production environment to production, unchanged | M |
| 4 · Contracted | As level 3 | — |

## Feature kill switches

Risky behaviour can be turned off without a deploy.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Risky features sit behind a configuration flag that can be flipped without shipping code | S |
| 3 · Critical | Flags are managed centrally with a record of who changed what; stale flags are removed rather than accumulating | M |
| 4 · Contracted | Gradual rollout and instant kill are supported per tenant or cohort | M |

## Automated versioning

Version numbers come from the commit history rather than someone remembering to bump a file.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | — | |
| 3 · Critical | The version is derived automatically at release time and tagged in the repository | M |
| 4 · Contracted | As level 3 | — |

## Automated release notes

What changed in a release is generated, not written from memory.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | — | |
| 3 · Critical | Release notes or a changelog are generated on release from commit history | S |
| 4 · Contracted | Notes distinguish user-visible changes from internal ones, and breaking changes are called out explicitly | M |
