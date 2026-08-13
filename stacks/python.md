# Python

**Detect:** `pyproject.toml`, `requirements.txt`, `setup.py`, `uv.lock`/`poetry.lock`

Stack-specific guidance per standard. Section names match standard names in `standards.md` exactly.

## Pre-commit hooks

`pre-commit` framework with ruff (lint + format); config committed. Leave mypy to pre-push or CI — it's too slow for a commit hook.

## Pinned tool versions

`.python-version` committed; dependencies locked (`uv.lock`/`poetry.lock`); tool versions pinned in `pyproject.toml`.

## Formatting

ruff format (or black), config in `pyproject.toml`, checked in CI.

## Type safety

mypy (or pyright) in strict mode on new code; run in CI.

## Linting

ruff with a meaningful rule set (not just defaults), zero errors in CI.

## Dependency vulnerability scanning

`pip-audit` or Dependabot/Renovate; runs in CI.

## Standard task names

Task runner (`justfile`, `Makefile`, or `poe`/`hatch` scripts) exposing `dev`, `test`, `lint`, `format`, `typecheck`.

## Validated configuration at startup

Settings parsed into a typed object at startup (pydantic-settings) with clear errors on missing/invalid vars.

## Secure coding patterns

Validate external input with pydantic at boundaries; parameterised queries via the ORM/driver only — no string-built SQL.

## Integration testing

pytest against real dependencies: testcontainers or a CI service DB; HTTP handlers exercised via the framework test client.
