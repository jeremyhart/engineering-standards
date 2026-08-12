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

[skills/standards-audit/](skills/standards-audit/) is a self-contained agent skill — the checklist and stack facets are embedded under its `references/` folder, so you can copy the folder anywhere and it still works. It grades a repo against the standards, marking each item met / missing / unknown / not applicable, and orders the gaps by urgency for your target stage.

### Install

Install with [`skills`](https://github.com/vercel-labs/skills), which pulls the skill from this repo into your agent's skills directory:

user-level, for all your agents (Claude Code, Copilot, Codex, …)
```bash
npx skills add jeremyhart/engineering-standards --global
```
or project-level, committed with the repo you want to audit
```bash
npx skills add jeremyhart/engineering-standards
```

### Run

Open the repo you want to audit and ask your agent, in natural language:

- "Audit this repo against the engineering standards."
- "Are we ready for prod?" — targets the `Prod` stage
- "Audit for `Internal` and tell me what to fix first."

The skill detects the target stage, project type, and applicable stack facets itself, states them as assumptions, and produces a report ranked by urgency. Pass a stage (`Prototype` / `Internal` / `Prod`) or a specific category to narrow the focus.

## Versioning

Releases follow semver. See [VERSION](VERSION) for the current release.
