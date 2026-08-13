# Development Workflow

The ordering and gating of work: intent stated before implementation, at three altitudes. This
category owns *sequence*. The artifacts themselves belong elsewhere — tests in Testing, decision
records in Documentation.

## Spec-driven development

What a change should do is agreed before it is built, and the agreement is written down where the
code can be checked against it.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Non-trivial work starts from a written intent — even a paragraph in the issue — rather than straight into the editor | S |
| 2 · Shared | Specs live in the repository alongside the code, with acceptance criteria stated in terms a reviewer can check | M |
| 3 · Critical | Specs are reviewed before implementation starts, and a behaviour change traces back to a spec change — the spec is not written afterwards to match what was built | M |
| 4 · Contracted | Specs cover the contractual surface (what customers are promised) and changes to that surface are approved before implementation | M |

## Test-driven development

Behaviour is expressed as a test before it is implemented.

Strictly, TDD isn't auditable — nothing in a repository shows whether the test came first. So this
control is graded on what *is* checkable, and what non-TDD codebases reliably fail: **every
behaviour change ships with a test change**.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | New behaviour arrives with tests in the same change, not "tests later" | M |
| 2 · Shared | The pattern is visible in history — test and implementation files move together in PRs; a behaviour change with no test change is questioned in review | M |
| 3 · Critical | Enforced by a diff-coverage gate in CI: changed lines must be covered, and the gate blocks the merge | M |
| 4 · Contracted | The red-green-refactor loop is documented as the expected workflow, and acceptance criteria from the spec map to named tests | M |

## Spec-first agent workflow

Non-trivial agent work goes through align → spec → tasks → implement → review, rather than a
one-shot prompt.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Agents are given a written brief for non-trivial work rather than an off-the-cuff instruction | S |
| 3 · Critical | The loop is documented and followed: agree the approach, write the spec, break it into tasks, implement, review — with a human checkpoint before implementation | M |
| 4 · Contracted | Agent-produced changes to the contractual surface go through the same approval as any other change to it | M |
