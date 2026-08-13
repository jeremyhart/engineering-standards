# Levels

Every standard is defined at four levels. A project sits at one of them, and each control tells you
what it looks like at that level. There is no single "done" — a control is met at your level or it
isn't.

## Rating a project

Ask four questions and take the **worst** answer. The rating is about consequence, not size, user
count, or how much work went into it.

1. **Disruption** — who is blocked when it's down, and can they work around it?
2. **Disclosure** — how bad if the data leaks?
3. **Loss** — how bad if the data is destroyed or corrupted? Is this the only copy?
4. **Obligation** — does anyone outside hold you to it by agreement?

| Level | Test |
|---|---|
| **1 · Personal** | Nothing is at stake but your own time — data is re-derivable, nobody else is blocked |
| **2 · Shared** | Other people depend on it; a day's outage annoys rather than stops, and data loss is recoverable with effort |
| **3 · Critical** | The business stops, or the only copy of something important is gone |
| **4 · Contracted** | Customers hold you to it by agreement — SLAs, tenancy, deletion requests, audits |

### The names describe audience; the rating does not

A one-person tool holding irreplaceable or sensitive data is **Critical**, not Personal. A
freelancer's invoicing app with client details has one user and rates 3 — on disclosure *and* on
loss. Rate by the four questions, not by who logs in.

### Worked examples

| Level | Examples |
|---|---|
| 1 | Hobby app with no real users; a prototype you'd cheerfully delete; a personal tool whose data can be re-derived |
| 2 | Team dashboard; internal Slack bot; marketing site; docs site; friendly beta; internal admin tool over data backed up elsewhere |
| 3 | Warehouse management system; ecommerce; EPOS; payroll; internal auth service; **any system of record whose loss can't be undone** |
| 4 | SaaS sold under contract; a public API others build on; payment processing operated for others; regulated multi-tenant platforms |

A warehouse system is used only by staff, holds no personal data, and still rates 3: when it's down,
the warehouse stops. That's the disruption question doing its job.

## Deciding what to implement

> **Implement every applicable control at `max(impact, desired maturity)`.**

**Impact** is the rating above — the floor, set by consequence. **Desired maturity** is ambition: you
may choose to run a level 2 project to level 3 standards because you expect it to grow, or because
the team wants the practice. Impact can only ever raise the bar, never lower it.

Re-ask the rating as the project changes. A prototype that quietly acquired real users rates 3 on
Friday having rated 1 on Monday, with no code change — and it now owes a great deal more.

## Reading a control

Each control in [`sections/`](sections/) carries a definition per level, an effort estimate per
level, and sometimes:

- **Applies when** — a precondition. If it doesn't hold, the control is not applicable, and an audit
  must cite the failed precondition rather than silently skipping it.
- **Requires** — another control that must be in place first. Implementing this one without its
  prerequisite is worse than not implementing it. An audit reports such a control as *blocked*, not
  *missing*.
- **Binary** — some controls are on or off (branch protection, a committed lockfile). These say so
  rather than inventing gradations.

Levels are cumulative: level 3 assumes level 2 is in place, and level 4 assumes both. Where a
control needs nothing more at a higher level, it says *as level 3* rather than padding.

Where a control has no definition at a level, it isn't expected yet — that is the control's minimum.

## Scope

This standard applies to **applications and services**: something with a repository, a deployment,
and users other than the process that started it.

Out of scope: one-off scripts, notebooks, throwaway proofs of concept, and personal automation. They
fail most categories for reasons that say nothing about their quality. A migration or script *inside*
an in-scope application is covered by that application's controls.
