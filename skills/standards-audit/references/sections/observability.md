# Observability

Seeing inside the running system. Responding to what you see belongs to Operations.

## Structured logging

Logs are machine-readable and consistent, so they can be searched rather than read.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | The application logs meaningfully — errors and key events, not just stack traces on crash | S |
| 2 · Shared | Logs are structured (JSON or equivalent) with consistent field names and real severity levels | S |
| 3 · Critical | Logs are shipped off the host to somewhere queryable and retained long enough to investigate an incident from last week | M |
| 4 · Contracted | Retention is defined and enforced per data class, and the retention period is one you could state to a customer | M |

## Log hygiene

Logs don't become the place your personal data quietly lives forever.

**Applies when:** the project handles personal data.

Logs are the most common leak of a deletion path: a record is deleted from the database and lives on
in six months of application logs. Audit logs are different — they record *events* (who did what,
when), not subject content, and keep their own retention policy.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | No credentials, tokens or full personal records in logs; identifiers stay out of URLs and query strings where they'd be captured by every intermediary | S |
| 3 · Critical | Redaction is applied at the logger with a field deny-list, so a careless log line can't leak; a retention window is set and enforced | M |
| 4 · Contracted | Log retention is part of the stated data-handling position, and deletion requests account for logs and backups as well as the database | M |

## Request correlation

A single request can be followed across everything it touched.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Every log line carries a request or trace identifier, so one request's lines can be pulled together | S |
| 3 · Critical | The identifier propagates across service boundaries and into background jobs | M |
| 4 · Contracted | Distributed tracing with spans and timings across services, sampled in production | L |

## Error tracking

Unhandled errors reach somewhere a human will see them.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Errors are visible in logs and you look when something seems wrong | S |
| 2 · Shared | Unhandled errors are captured and reported to an error tracker with stack trace and context | S |
| 3 · Critical | Errors are grouped, assigned and triaged rather than accumulating; regressions are noticed because a resolved error reopening raises attention | M |
| 4 · Contracted | Error rates feed the service-level objectives, and a spike is an incident rather than a chart | M |

## Health endpoint

The system can be asked whether it is working.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | An endpoint reports whether the app is up, and it's used by the deploy check | S |
| 3 · Critical | Liveness and readiness are distinguished, dependencies are checked (database, queue, upstreams), and the endpoint is monitored continuously from outside | M |
| 4 · Contracted | Health is monitored from more than one location, and failures are reflected in status communication | M |

## Alerting

Something tells a human when the system needs attention, and that alert is worth waking for.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Alerts exist for the failures that matter — the app being down, error rates spiking — and they reach someone | M |
| 3 · Critical | Every alert names an owner and links a runbook. Alerts that fire without needing action are removed or retuned, because an ignored alert is worse than none | M |
| 4 · Contracted | Alerting is driven by service-level objectives and error budgets rather than raw thresholds, so pages correspond to promises being at risk | L |
