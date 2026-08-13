# Operations

Running the system and responding when it misbehaves.

## Ownership and on-call

It is clear who is responsible, and who picks up the phone.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | A named owner for the service, and it's written down where someone looking for help would find it | S |
| 3 · Critical | Who responds out of hours is defined and agreed, with a documented escalation path when they don't answer | M |
| 4 · Contracted | A rota with formal escalation, and response expectations that match what customers are promised | M |

## Incident response process

There is a known path from "something is wrong" to "it's fixed and we learned something".

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | A written note: who to contact, where problems get raised, and how urgency is decided | S |
| 3 · Critical | A defined path — alert to owner to fix to review — with incidents recorded and severity levels that mean something | M |
| 4 · Contracted | Blameless postmortems for significant incidents, with actions tracked to completion, and customer communication timescales that match the agreement | M |

## Runbooks

The known failure modes have written responses, so recovery doesn't depend on who is awake.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | The two or three things that actually go wrong have written recovery steps | S |
| 3 · Critical | Runbooks cover each alert, are linked from the alert itself, and are accurate enough to follow at 3am without prior knowledge | M |
| 4 · Contracted | Runbooks are exercised and revised on a schedule, not just written once | M |

## Backup and restore

The data can be brought back, and you know that because you've done it.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Whatever can't be re-derived is backed up somehow, even manually | S |
| 2 · Shared | Backups are automated and their success is monitored — a silently failing backup job is the common failure here | M |
| 3 · Critical | The restore procedure is documented and has been performed successfully, with the time it took recorded. Backups are stored separately from the thing they protect | M |
| 4 · Contracted | Recovery time and recovery point objectives are stated, and a full disaster-recovery restore is rehearsed on a schedule against those numbers | L |

## Status communication

Users can find out that the service is degraded without asking you.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | A place where a known problem is announced — a pinned message or channel is enough | S |
| 3 · Critical | Users are told about degradation without contacting anyone, and told again when it's resolved | M |
| 4 · Contracted | An externally hosted status page, updated within the timescales the agreement specifies, with incident history retained | M |

## Cost monitoring

Spend is visible before it becomes a surprise.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | A budget alert exists so a runaway resource can't quietly bill for a month | S |
| 2 · Shared | Spend is reviewed periodically and attributed to the service | S |
| 3 · Critical | Alerts on anomalies as well as totals, since a cost spike is often the first sign of abuse or a loop | M |
| 4 · Contracted | Cost per tenant or per unit of work is understood well enough to price and to notice a customer becoming unprofitable | M |
