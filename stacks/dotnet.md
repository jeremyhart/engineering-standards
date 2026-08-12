# .NET (C# / ASP.NET Core)

**Detect:** `*.csproj`, `*.sln`, `global.json`, `Directory.Build.props`

Stack-specific guidance per standard. Section names match standard names in `standards.md` exactly.

## Pinned tool versions

`global.json` pins the SDK version; `Directory.Packages.props` for central package management; lockfiles (`packages.lock.json`) where supply-chain matters.

## Formatting

`dotnet format` with `.editorconfig` committed; verified in CI (`dotnet format --verify-no-changes`).

## Typecheck

`<Nullable>enable</Nullable>` and `<TreatWarningsAsErrors>true</TreatWarningsAsErrors>` in a shared `Directory.Build.props`.

## Linting

Roslyn analyzers enabled (`<AnalysisLevel>latest-recommended</AnalysisLevel>`); warnings fail the build in CI.

## Dependency vuln scanning

`dotnet list package --vulnerable` in CI, or Dependabot/Renovate for NuGet.

## Standard task names

Task runner (`justfile`/`Makefile` or documented `dotnet` commands) exposing `dev`, `build`, `test`, `lint`, `format`.

## `.env.example`

Options pattern with `ValidateDataAnnotations().ValidateOnStart()` so missing/invalid config fails at startup, not first use.

## Migrations

EF Core migrations committed; no manual schema edits. `dotnet ef migrations bundle` (or script) for pipeline apply.

## Secure coding baseline

Model binding + FluentValidation/DataAnnotations at boundaries; EF/parameterised queries only — no string-built SQL.

## Structured logging

`ILogger` with structured message templates (no string interpolation); Serilog or OpenTelemetry exporter configured.

## Request correlation

W3C trace context propagation via built-in `ActivitySource`/OpenTelemetry; correlation ID on every log scope.

## Health endpoint

ASP.NET Core health checks (`MapHealthChecks`) with dependency checks (DB, downstream services).

## Integration testing

`WebApplicationFactory` for in-process HTTP tests; testcontainers for real DB dependencies.
