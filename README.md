# Software Project Standards

The standards I hold a software project to, graded across four levels — from a personal tool to
something customers hold you to by contract.

- **[standards.md](standards.md)** — the index: every standard, by category
- **[levels.md](levels.md)** — the four levels, and how to rate a project
- **[sections/](sections/)** — what each standard means at each level, with the effort it takes
- **[stacks/](stacks/)** — per-stack tooling for the standards that have an opinion

## How it works

Rate the project by consequence, not size — the worst answer across four questions: who is blocked
when it's down, how bad if the data leaks, how bad if it's destroyed, and whether anyone holds you to
it by agreement.

| Level | |
|---|---|
| **1 · Personal** | Nothing at stake but your own time |
| **2 · Shared** | Other people depend on it |
| **3 · Critical** | The business stops, or the only copy of something important is gone |
| **4 · Contracted** | Customers hold you to it — SLAs, tenancy, deletion requests, audits |

Then implement every applicable control at `max(impact, desired maturity)` — impact sets the floor,
ambition can raise it.

A one-person tool holding irreplaceable data is Critical, not Personal. The names describe audience;
the rating doesn't.

## Audit skill

A skill that audits a codebase against the standards and produces a report.

```bash
npx skills add jeremyhart/engineering-standards --global
```

Open the repo you want to audit and ask your agent, in natural language:

- "Audit this repo against the standards."
- "Are we ready for prod?" — rates the project and grades it at the level its impact demands
- "Audit at `Critical` and tell me what to fix first."

The skill rates the project, detects its type and stack facets, states all of that as assumptions,
and reports what's below the required level — ranked by how far below, then by effort. Pass a level
(`Personal` / `Shared` / `Critical` / `Contracted`) or a category to narrow the focus.
