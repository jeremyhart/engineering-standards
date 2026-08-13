# Documentation

## README completeness

The README answers, in order: what this is, how to run it, how to deploy it, and how it's put
together.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Purpose and run: what the project does, who it's for, its status, and how to run it locally — in the first screenful | S |
| 2 · Shared | Adds deploy: how each environment is deployed and where it lives | S |
| 3 · Critical | Adds architecture: a diagram or description of the components and how they talk, enough for a newcomer to reason about a change | M |
| 4 · Contracted | Adds the operational surface: supported versions, support expectations, and where status and incidents are communicated | M |

## Architecture decision records

Significant decisions are recorded with their context and consequences, so the reasoning survives
the people who were there.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Significant or surprising decisions are written down somewhere durable, with why the alternatives were rejected | M |
| 3 · Critical | ADRs are kept in the repo in a consistent format, dated, and linked from the project context so agents and newcomers find them | M |
| 4 · Contracted | As level 3 | — |

## Infrastructure documentation

What exists, where it lives, and who can reach it — recorded or generated from the infrastructure
code.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Resources, accounts and regions are listed somewhere other than one person's memory | S |
| 3 · Critical | Topology and access are documented: what talks to what, which identities are used, and how access is granted. Generated from infrastructure code where possible | M |
| 4 · Contracted | Includes data flows and where customer data resides, kept current as part of change review | M |

## Docs drift detection

Documentation that has drifted from the code is found automatically, and the deviation becomes work
rather than a discovery months later.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Docs are reviewed at release: the person shipping checks whether what they changed invalidated anything written down | S |
| 3 · Critical | A scheduled check compares docs against the code and reports deviations — typically a skill invoked by a scheduled CI workflow | L |
| 4 · Contracted | Deviations are filed automatically as tickets and tracked to closure, so drift can't be ignored quietly | L |
