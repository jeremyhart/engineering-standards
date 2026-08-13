# Code Quality

Static checks on the code itself. All three are enforced in the same three places — agent post-edit
hook, pre-commit, CI — with CI authoritative. See *Pre-commit hooks* in Source Control & Review for
the ordering rule.

## Linting

A linter is configured, its findings are acted on, and it is enforced rather than advisory.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | A linter is installed with a committed config and runs from a standard task name | S |
| 2 · Shared | Zero errors in the codebase; the linter runs in CI and fails the build | S |
| 3 · Critical | Rules cover correctness and security patterns, not only style; blanket disables carry a justifying comment | M |
| 4 · Contracted | As level 3 | — |

## Formatting

Formatting is automated and not a matter of opinion or review comments.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | A formatter is configured with committed settings | S |
| 2 · Shared | Formatting is enforced in CI; the whole codebase is formatted, so diffs stay meaningful | S |
| 3 · Critical | As level 2 | — |
| 4 · Contracted | As level 2 | — |

## Type safety

Types are checked automatically, and the strictness rises with the cost of being wrong.

**Applies when:** the language has a type checker, whether built in or as a supported add-on. On a
language with no type checking available, this control is not applicable — the audit says so and
cites this precondition.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | A type checker runs and the project passes it under default settings | S |
| 2 · Shared | Zero type errors, enforced in CI; escape hatches (`any`, casts, ignore comments) are the exception rather than the pattern | M |
| 3 · Critical | Strictness raised beyond defaults — null-safety and implicit-any equivalents enabled; new escape hatches need a justifying comment | M |
| 4 · Contracted | As level 3 | — |
