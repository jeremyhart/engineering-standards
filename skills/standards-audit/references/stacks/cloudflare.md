# Cloudflare

**Detect:** `wrangler.jsonc`/`wrangler.toml`, `workers-site/`, `@cloudflare/*` dependencies

Stack-specific guidance per standard. Section names match standard names in `standards.md` exactly.

## Infrastructure as code

`wrangler.jsonc` is the source of truth for Worker config (bindings, routes, vars); dashboard-only config avoided. Terraform/Pulumi for zone-level resources.

## Hosting details documented

Account ID, zone, Worker names, KV/D1/R2/Queue binding names recorded.

## Runtime secrets management

`wrangler secret put` / Secrets Store; never `vars` in wrangler config or `.dev.vars` committed.

## Automated deployment

Workers Builds or CI running `wrangler deploy`; no deploys from dev machines.

## Deployment validation

Post-deploy smoke test against the deployed Worker; `wrangler versions` gradual rollout where appropriate.

## Rollback strategy

`wrangler rollback` / versions; procedure documented and tested.

## Structured logging

Workers Logs / Logpush enabled; `console.log` with structured objects; `observability` enabled in wrangler config.

## Error tracking

Workers Logs or Sentry/Tail Workers for unhandled exceptions.

## Health endpoint

Health route on the Worker; monitored externally (Workers have no liveness probes).

## Request correlation

Log the `cf-ray` header (or generate a request ID) on every log line; forward it on outbound subrequests and service bindings.

## Rate limiting and abuse protection

Cloudflare WAF + Rate Limiting rules at the zone; Workers Rate Limiting binding or Durable Object counters for per-user quotas.

## Feature kill switches

Env vars in `wrangler.jsonc` per environment as the minimum; KV-backed flags or a flag service for no-deploy toggles.

## Cost monitoring

Usage-based billing: alerts on request volume/CPU-time anomalies; paid-tier limits reviewed.
