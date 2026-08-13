# API & Contracts

The dividing line between these two controls is **can you deploy both sides at once?** If you can,
the contract's job is catching drift. If you can't, the contract is a promise, and changing it costs
a version, a window, and a conversation.

## Interface contract

The shape of the API is described somewhere both sides derive from, so client and server can't drift
apart silently.

Applies where you control both ends — a frontend and your own backend, or services you release
together. If a change is needed, you change both sides in one PR.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Client and server share types or a generated client rather than hand-written duplicates on each side | M |
| 3 · Critical | The API surface is described by a machine-readable contract (schema or IDL) that the implementation is checked against | M |
| 4 · Contracted | Breaking changes to the contract are detected in CI and fail the build | M |

## Published API contract

The API is a promise to consumers you cannot deploy alongside, so changes are versioned, announced,
and supported for a stated period.

**Applies when:** the API has consumers you can't release with — customers, partners, or another
team's service on its own release cycle.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | — | |
| 3 · Critical | The API is versioned, and breaking changes ship as a new version rather than mutating the existing one | M |
| 4 · Contracted | A deprecation policy with a stated support window; changes announced before they land; old and new versions run concurrently through the window; consumer contract tests guard the promise | L |
