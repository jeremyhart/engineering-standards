# Source Control & Review

Version control and the collaboration process around it.

## Version control

Code is managed in a version control system.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | All code lives in a repository with readable history | S |
| 2 · Shared | Repository is shared; branching and merging workflows are defined | S |
| 3 · Critical | As level 2 | — |
| 4 · Contracted | As level 2 | — |

## Branch protection

The main branch can't be changed by accident.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Work happens on branches and lands via pull request, so history stays reviewable — self-merge is fine when you're the only maintainer | S |
| 2 · Shared | Main is protected: no direct pushes, no force pushes, and required checks must pass before merge | S |
| 3 · Critical | Protection applies to everyone including administrators, and history is not rewritten after the fact | S |
| 4 · Contracted | As level 3 | — |

## Code review process

Changes are looked at by someone other than their author — where someone else exists.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Changes go through a pull request with green checks; approval by another human is not required when there isn't one | S |
| 2 · Shared | Review is expected for non-trivial changes, and what reviewers are looking for is written down | S |
| 3 · Critical | At least one approving review from someone other than the author is required to merge, with CODEOWNERS routing changes to the people who know that area | S |
| 4 · Contracted | Review of sensitive areas is enforced by ownership rules, and the review record is retained as evidence | S |

## Pre-commit hooks

Fast checks run before a commit is made, so obvious problems never enter history.

**The ordering rule.** Three places run these checks and they have different jobs: the agent
post-edit hook fixes as it goes, pre-commit is a fast gate, and **CI is authoritative** — nothing
merges without it. Pre-commit must stay under a few seconds: formatting and cheap lint only. Type
checking and tests belong at pre-push or in CI. Hooks that make committing slow are hooks that get
bypassed.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Formatting and fast lint run on staged files before commit | S |
| 3 · Critical | Hooks are installed automatically as part of setup, so they apply to everyone rather than the people who remembered | S |
| 4 · Contracted | As level 3 | — |

## Commit naming schema

Commit messages follow a convention, so history is readable and releases can be derived from it.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | A convention is agreed and followed — messages say what changed and why | S |
| 3 · Critical | The convention is enforced by tooling, which is what makes automated versioning and release notes possible | S |
| 4 · Contracted | As level 3 | — |

## PR template

Pull requests arrive with the context a reviewer needs.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | A template prompts for what changed, why, and how it was tested | S |
| 3 · Critical | The template includes the checks the project cares about — migrations, config changes, rollback considerations | S |
| 4 · Contracted | As level 3 | — |
