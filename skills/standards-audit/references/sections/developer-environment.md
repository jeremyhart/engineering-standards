# Developer Environment

## Setup docs and script

Someone with a clean machine can go from clone to running application by following one documented
path.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | The README says how to install dependencies and start the app, and the instructions actually work | S |
| 2 · Shared | A single setup script does it, and a new contributor is running within an hour without asking questions | M |
| 3 · Critical | Setup is reproducible on a clean machine — no undocumented local state, no "ask someone for the file" | M |
| 4 · Contracted | As level 3 | — |

## Standard task names

Common operations have predictable entry points, so people and agents don't have to learn this
project's private vocabulary.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Running, building and testing each have a task-runner entry point rather than a remembered command line | S |
| 2 · Shared | The standard set exists and is documented: `dev`, `build`, `test`, `lint`, `format`, `typecheck` — same names as everywhere else | S |
| 3 · Critical | CI invokes the same entry points developers do, so local and pipeline behaviour can't diverge | S |
| 4 · Contracted | As level 3 | — |

## Documented configuration

Every configuration value the application needs is listed, with an example, before anyone has to
discover it from a crash.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | An example configuration file lists every required variable | S |
| 2 · Shared | Each variable is described — what it does, what a valid value looks like, whether it's optional — and the example is kept current | S |
| 3 · Critical | Secrets are named but never valued in the example; where each real value comes from is documented | S |
| 4 · Contracted | As level 3 | — |

## Validated configuration at startup

Missing or malformed configuration fails immediately and says what's wrong, rather than surfacing as
a confusing error later.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Configuration is read and validated once at startup; a missing required value stops the process with a clear message naming it | S |
| 3 · Critical | Values are parsed into their real shapes (numbers, URLs, enums) and validated, not passed around as raw strings | M |
| 4 · Contracted | As level 3 | — |

## Pinned tool versions

The toolchain is fixed, so builds don't change because someone upgraded something.

**Binary** — a version is pinned or it isn't.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | The dependency lockfile is committed | S |
| 2 · Shared | Runtime and toolchain versions are pinned in a file the tooling reads, and CI uses the same versions | S |
| 3 · Critical | As level 2 | — |
| 4 · Contracted | As level 2 | — |

## Seeding script

A repeatable script fills a local database with representative data.

**Applies when:** the project has a database.

For *test* data, prefer per-test factories over a shared seeded dataset — this control is about
getting a usable development environment, not about fixtures.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | A script populates a local database with enough realistic data to exercise the main paths | M |
| 3 · Critical | Seed data is representative of production shape and volume, and contains no copied production personal data | M |
| 4 · Contracted | As level 3 | — |
