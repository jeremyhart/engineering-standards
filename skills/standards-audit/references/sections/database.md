# Database

*Only relevant if the project has a database.*

The unifying idea: **nobody changes the data model by hand**. Everything arrives through code,
reviewed and repeatable, and at higher levels humans don't hold the access that would let them do
otherwise.

## Database changes only through code

Schema and data changes ship as versioned migrations in the repository, reviewed like any other
code. Nobody edits a schema through a console.

**For stores without a fixed schema** the obligation is the same, not lighter: model changes and
backfills are versioned scripts applied by tooling, so any environment can be brought to a known
shape. Only where there is no data model to change at all does this control not apply.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Schema changes exist as migration files in the repo — not as commands typed into a console and forgotten | M |
| 2 · Shared | No manual edits in any environment; migrations are reviewed as part of the PR that needs them | M |
| 3 · Critical | Developers have no standing write access to production data or schema; changes arrive only through the pipeline | M |
| 4 · Contracted | Break-glass access only: time-boxed, approved, logged, and reviewed afterwards | M |

## Migrations in pipeline

Migrations are applied automatically by the deploy pipeline, before the code that depends on them.

**Requires:** *Non-destructive migrations* at level 3 and above — automatic migration plus automatic
rollback is only survivable if migrations are backward-compatible.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Migrations run from a documented command, not from someone's laptop against production | S |
| 3 · Critical | The pipeline applies migrations before deploying the code, with no human step | M |
| 4 · Contracted | Migration failures halt the deployment and alert, leaving the previous version serving | M |

## Migrations rehearsed before production

Every migration runs somewhere else first, against realistic data, before it touches production.

**Requires:** *Non-production environment* (Infrastructure 3).

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Migrations are run locally against a representative dataset before release | S |
| 3 · Critical | Every migration runs in a non-production environment before production, as part of the pipeline | S |
| 4 · Contracted | Rehearsal uses a production-sized, masked dataset, and the run time is measured — a migration that locks a large table is caught here, not in production | M |

## Non-destructive migrations

Migrations never drop or rename in a way that breaks the currently running version. Expand first,
migrate, then contract in a later release.

This is what makes rollback safe. Without it, restoring the previous app version leaves old code
against a new schema.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Destructive changes are done knowingly, with the deploy sequenced so the running version isn't broken | M |
| 3 · Critical | Expand–contract is the standard pattern: add the new shape, backfill, switch the code, remove the old shape in a later release | M |
| 4 · Contracted | As level 3 | — |

## Forward-only migrations

There are no down migrations. Recovery is restoring the previous application version, not reversing
the schema.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | — | |
| 3 · Critical | Down migrations are not written or relied on; the documented recovery path is redeploying the previous version against the current schema | S |
| 4 · Contracted | As level 3 | — |
