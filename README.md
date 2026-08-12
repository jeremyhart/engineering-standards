# engineering-standards

A stage-gated engineering checklist — practical rather than aspirational. Every item is tagged with the earliest project stage it matters for, so a prototype and a production service can share one checklist without being held to the same bar.

## What's here

- [standards.md](standards.md) — the checklist itself. One row per standard, each tagged with the risk of skipping it, the earliest stage it matters (`Prototype` / `Internal` / `Prod`), and rough adoption effort. Rows are stack-neutral.
- [stacks/](stacks) — per-technology guidance (TypeScript, Cloudflare, Azure, Docker, Python, and more). A project composes several facets; each maps the neutral standards to concrete tooling.
- [skills/standards-audit/](skills/standards-audit/) — a Copilot/Claude skill that grades a real repo against the checklist, with evidence, and tells you what to fix first for a given target stage.

## Using the checklist

Pick your target stage, then read [standards.md](standards.md) and skip anything above that stage. Categories carry an *"Applies when: ..."* line where they only apply under certain conditions — skip those that don't hold.

Pull in the [stacks/](stacks) files that match your technology to turn each neutral standard into concrete tooling.

## Using the audit skill

Copy `skills/standards-audit/` into your agent's skills folder. Then ask your agent to audit the current repo against the standards for a given stage; it grades each item as met, missing, unknown, or not applicable, and orders the gaps by urgency. The skill is self-contained — the checklist and stack facets are embedded under its `references/` folder.

## Versioning

Releases follow semver. See [VERSION](VERSION) for the current release.
