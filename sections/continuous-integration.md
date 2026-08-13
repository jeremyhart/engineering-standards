# Continuous Integration

CI owns the gate and the build. Security scanning lives in Security even though it runs here —
controls are filed by concern, not by the pipeline that happens to execute them.

## PR checks required before merge

The build, the checks and the tests run on every change, and failing them prevents a merge.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | CI runs build, lint and tests on push, and you look at the result | S |
| 2 · Shared | Checks are *required* — the merge button is blocked until they pass, not merely red in the UI | S |
| 3 · Critical | The full check set (build, lint, types, tests) is required; CI runs on a clean checkout so a green local machine can't mask a broken build | S |
| 4 · Contracted | As level 3 | — |

## Dependency update policy

There is a stated cadence for taking dependency updates, so security fixes don't queue behind
neglect.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | An update bot is enabled and its PRs are merged rather than left to rot | S |
| 3 · Critical | A written cadence for reviewing and merging update PRs, and a stated approach to major versions | S |
| 4 · Contracted | As level 3 | — |

## Build provenance

You can say what a shipped artifact was built from and prove it wasn't altered afterwards.

Lower levels are covered by *Pinned tool versions* and a committed lockfile in Developer Environment.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | — | |
| 3 · Critical | Builds are reproducible from a tagged commit; the artifact records the commit it came from | M |
| 4 · Contracted | An SBOM is produced and published for each release, and artifacts are signed so consumers can verify provenance | L |
