# Testing

The test suite as an asset: what exists and what it covers. *When* tests are written belongs to
Development Workflow.

## Unit testing

The logic that matters is covered by fast tests that run on every change.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | The core logic has tests, and they run with one command | M |
| 2 · Shared | Tests run in CI and failures block the merge; the suite is fast enough that people run it locally | M |
| 3 · Critical | Coverage of the logic that carries risk is deliberate rather than incidental, and each test isolates its own data instead of depending on shared mutable state | M |
| 4 · Contracted | As level 3 | — |

## Integration testing

The seams are tested against real dependencies, not mocks of your own assumptions.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Database queries and HTTP handlers are exercised against a real database and a real server, not stubs | M |
| 3 · Critical | Integration tests run in CI against ephemeral instances of the real dependencies, and cover the failure paths as well as the happy ones | L |
| 4 · Contracted | Contract tests cover the boundaries with anything you can't deploy alongside | L |

## End-to-end testing

The journeys that matter are exercised the way a user would.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | — | |
| 3 · Critical | The critical user flows — the ones whose failure means the business stops — are covered and run in CI or on a schedule | L |
| 4 · Contracted | Flows are also run against production after deploy as the post-deploy validation, so a broken journey is caught before a customer reports it | L |
