# Docker

**Detect:** `Dockerfile`, `compose.yaml`/`docker-compose.yml`

Stack-specific guidance per standard. Section names match standard names in `standards.md` exactly.

## Pinned tool versions

Base images pinned to digest or exact tag (no `latest`).

## Artifact promotion

Image built once, tagged immutably (commit SHA), promoted staging → prod by retag — never rebuilt.

## Health endpoint

`HEALTHCHECK` in Dockerfile / orchestrator probes wired to the app's health endpoint.

## Setup docs and script

`compose up` brings up the full local stack (app + DB + dependencies) in one command.

## Dependency vulnerability scanning

Image scanning (Trivy/Grype or registry-native) in CI; blocks on critical findings.

## Runtime secrets management

Secrets injected at runtime (env/secret mounts); never baked into image layers or committed compose files.

## Integration testing

testcontainers (or compose-managed services in CI) provide real DB/queue dependencies for integration tests.
