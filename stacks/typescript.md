# TypeScript

**Detect:** `tsconfig.json`, `package.json`

Stack-specific guidance per standard. Section names match standard names in `standards.md` exactly.

## Pre-commit hooks

Husky + lint-staged. Prettier on staged files; `tsc --noEmit` and tests on commit.

## Pinned tool versions

`.nvmrc` and `engines` specified; lockfile committed; TypeScript pinned exactly (no `^`).

## Formatting

Prettier, config committed, `format` and `format:check` scripts.

## Typecheck

`"strict": true` in tsconfig; consider `noUncheckedIndexedAccess`. `typecheck` script runs `tsc --noEmit` in CI.

## Linting

ESLint with `typescript-eslint`, zero errors, `lint` script enforced in CI.

## Dependency vuln scanning

Dependabot or Renovate; `npm audit` (or `pnpm audit`) step in CI.

## Automated versioning

semantic-release or changesets driven by Conventional Commits.

## Standard task names

npm scripts: `dev`, `build`, `test`, `lint`, `format`, `typecheck`.

## `.env.example`

Parse env into a typed object at startup (e.g. zod) with clear errors on missing/invalid vars.

## API contract defined

tRPC or Hono RPC for end-to-end types with a TS client; otherwise OpenAPI generated from zod schemas (`zod-openapi`) with breaking-change diff in CI.

## Secure coding baseline

Validate all external input with zod at boundaries; parameterised queries via the ORM/query builder only — no string-built SQL.

## Integration testing

Vitest against real dependencies: testcontainers or a CI service DB; HTTP handlers exercised via `app.request()`/supertest, not mocked.
