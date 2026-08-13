# Security

Filed by concern, not by mechanism — secret and dependency scanning live here even though they run
in the pipeline.

## Runtime secrets management

Secrets live in a managed store, not in the repository and not in configuration files.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | No secrets in the repository or in committed config; local secrets stay in an ignored file | S |
| 2 · Shared | Runtime secrets come from the platform's secret store or environment injection, never from a committed file | M |
| 3 · Critical | A managed secret store with workload identity — the application authenticates as itself rather than holding a long-lived credential. Access to secrets is restricted and logged | M |
| 4 · Contracted | Secrets are rotated on a schedule and on personnel change, and rotation is exercised rather than theoretical | M |

## Secret scanning

Committed secrets are found automatically, with a way to handle the findings that doesn't end in the
scanner being switched off.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | A scanner runs over the repository and you act on what it finds | S |
| 2 · Shared | Scanning runs in the pipeline on every change and reports findings | S |
| 3 · Critical | Findings block the merge, with a baseline file for accepted false positives; each suppression carries a reason and an expiry date | S |
| 4 · Contracted | History is scanned as well as the diff, and a real exposure triggers rotation, not just deletion of the line | M |

## Dependency vulnerability scanning

You know when something you depend on becomes dangerous — and when its licence becomes a problem.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | An audit command runs against dependencies occasionally | S |
| 2 · Shared | Scanning runs in the pipeline and reports; an update bot raises fixes | S |
| 3 · Critical | High and critical findings block the merge, with a suppression file carrying reasons and expiry dates. Licences are checked against an allowlist | M |
| 4 · Contracted | Remediation deadlines by severity, tracked to closure; licence violations block the build | M |

## Least-privilege access

Every identity has the narrowest access that lets it work.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | No shared administrative credentials; each person and service has its own identity | M |
| 3 · Critical | Access is scoped to what each identity needs, roles are documented, and service identities are separate from human ones | M |
| 4 · Contracted | Access is reviewed on a schedule with the review evidenced and retained, and removal on departure is part of a checklist | M |

## Production access control

Who can reach production is deliberate, known and revocable.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | It's known who can reach production, and it's a short list | S |
| 3 · Critical | Access is granted explicitly and reviewed; standing write access to production data is not the default working state | M |
| 4 · Contracted | Break-glass access only, time-boxed, approved and logged, with the log reviewed afterwards | M |

## Deny-by-default auth

Authentication is defined once and applied everywhere, and anything not explicitly public is
protected.

The property matters, not the pattern: auth is written once and enforced for every route, rather
than each endpoint remembering to check for itself. The facet files name the mechanism per stack —
framework middleware, a gateway policy, a filter pipeline.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Endpoints require authentication; public ones are the explicit exception rather than the accident | M |
| 3 · Critical | Enforcement is centralised in one mechanism the codebase can point to, and it fails closed — a new endpoint is protected by default, and a broken auth check denies rather than allows | M |
| 4 · Contracted | The public surface is enumerated and reviewed, and adding to it is a deliberate, reviewed act | M |

## Endpoint authorization

Being logged in is not the same as being allowed, and the difference is explicit.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Endpoints that act on someone's data check that it belongs to them | M |
| 3 · Critical | Authorization rules are explicit per endpoint and reviewed, following a consistent approach rather than ad-hoc checks scattered through handlers | M |
| 4 · Contracted | Tenant isolation is tested — an automated test proves one customer cannot reach another's data, and it runs on every change | L |

## Secure coding patterns

The code uses the patterns that make whole classes of vulnerability impossible, rather than relying
on care.

Audited by sampling, not proof: queries going through a parameterised API or ORM rather than string
building, a validation library used at boundaries, output encoded by the template layer. An audit
reports what it sampled rather than claiming exhaustive verification.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Queries are parameterised — no string-built SQL anywhere | S |
| 2 · Shared | Input is validated at the boundary against a schema, and output is encoded by default rather than by remembering | M |
| 3 · Critical | Static analysis runs in CI, with lint rules enforcing the patterns so a regression fails the build | M |
| 4 · Contracted | Third-party penetration testing on a stated cadence, with findings tracked to closure | L |

## Audit logging

Security-relevant events are recorded in a way that survives the person who caused them.

Audit logs record events — actor, action, target, time — not subject content. That is what lets them
be retained under their own policy while personal data elsewhere is deleted on request.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Logins, permission changes and data exports are logged with who and when | M |
| 3 · Critical | The audit log is separate from application logs and cannot be edited by the application that writes it; its retention is defined | M |
| 4 · Contracted | Retention meets the agreed period, the log is queryable for an audit, and access to it is itself controlled and logged | M |

## Personal data handling

You know what personal data you hold, where it is, and how to remove it.

**Applies when:** the project handles personal data.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | What personal data is held, and where, is documented | M |
| 3 · Critical | A deletion path is implemented and tested, and retention is enforced in code or schema rather than by intention. Logs and backups are accounted for | M |
| 4 · Contracted | Export and deletion are available on request within the agreed timescale, and both are exercised rather than assumed to work | L |

## Rate limiting and abuse protection

Public and expensive endpoints can't be used to exhaust you.

**Applies when:** the service is reachable by parties you don't control.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Platform-level rate limiting is enabled at the gateway or edge | S |
| 3 · Critical | Per-user and per-endpoint quotas on costly operations; abuse and anomalies raise an alert | M |
| 4 · Contracted | Quotas are per tenant and enforced against the agreement, with abuse response documented | M |
