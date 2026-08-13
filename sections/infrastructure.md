# Infrastructure

## Infrastructure as code

Infrastructure is defined in the repository and applied by tooling. Nothing important exists only
because someone clicked it into being.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | The main resources are defined in code and can be recreated from it; ad-hoc console changes are the exception and get folded back in | M |
| 3 · Critical | All infrastructure is defined in code; nobody creates production resources by hand, and state is stored centrally with locking | L |
| 4 · Contracted | Changes go through plan review before apply, and drift between code and reality is detected on a schedule | L |

## Hosting details documented

Where the thing actually runs is written down.

**Capture at minimum:** the cloud account or subscription and its identifier; the resource names and
their region; the DNS records and where the domain is registered; the source repository and pipeline;
any third-party service the app depends on, with the account it belongs to; and who holds the
billing relationship.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | The account, resource names and region are recorded somewhere findable | S |
| 2 · Shared | The full list above is documented and current, so someone else could find and reach everything without you | S |
| 3 · Critical | Kept current as part of change review; ownership of each account and service is named | S |
| 4 · Contracted | As level 3 | — |

## Non-production environment

There is somewhere other than production to try things.

Three other controls depend on this one: migration rehearsal, artifact promotion, and environment
parity.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Local development plus production, with the differences between them documented | S |
| 3 · Critical | A staging environment that mirrors production topology — same services, same shapes, same deploy path. Where the platform offers deployment slots, that is the cheapest way to get it, and it brings slot-swap rollback with it | M |
| 4 · Contracted | Staging carries a production-sized dataset with personal data masked, so performance and migration behaviour are representative | L |

## Environment parity

Environments differ only in ways you know about and have written down.

**Requires:** *Non-production environment* at level 3 and above.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | The differences between local and production are documented — versions, backing services, feature flags | S |
| 3 · Critical | Environments are provisioned from the same infrastructure code with parameters, not hand-built separately; differences are deliberate and listed | M |
| 4 · Contracted | Parity is verified rather than assumed — configuration differences between environments are reported, and unexplained ones are treated as defects | M |
