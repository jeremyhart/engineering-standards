# Azure

**Detect:** Bicep files, `azure.yaml`, ARM templates, Azure pipelines/workflows referencing `azure/login`

Stack-specific guidance per standard. Section names match standard names in `standards.md` exactly.

## IaC

Bicep (or Terraform azurerm). No portal-created resources; drift checked.

## Hosting details documented

Subscription ID, resource group, resource names & regions recorded.

## Runtime secrets management

Key Vault + managed identity; app settings reference Key Vault, no secrets in App Service config or repo.

## Least-privilege access

RBAC role assignments scoped to resource group or below; roles documented.

## Error tracking

Application Insights; unhandled exceptions and dependency failures captured.

## Structured logging

App Insights / Log Analytics with structured properties, not string logs.

## Alerting

Azure Monitor alert rules on failures and key thresholds; action groups configured.

## Cost monitoring

Budget + cost alerts on the subscription/resource group; spend reviewed regularly.

## Backup & restore procedure

Automated backups on the data tier (e.g. point-in-time restore); restore rehearsed.

## Request correlation

App Insights distributed tracing with W3C `traceparent` propagation; operation IDs on every log entry.

## Rate limiting & abuse protection

API Management rate-limit policies or Front Door/App Gateway WAF rules; per-key quotas in APIM for costly endpoints.

## Feature kill switches

App Configuration feature flags as the managed option; app settings as the minimum.

## Audit logging

Entra sign-in/audit logs plus resource-level diagnostic settings shipped to Log Analytics; app-level audit events as custom events.
