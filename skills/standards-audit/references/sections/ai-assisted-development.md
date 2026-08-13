# AI-Assisted Development

Building software with coding agents. These controls are about what agents can read, what they may
do, and how their configuration is governed — not about features that call an LLM at runtime.

## Agent-readable project context

The project explains itself to an agent without a human in the loop: architecture, conventions,
gotchas, and the vocabulary the codebase uses.

Graded by **content, not filenames**. `CONTEXT.md` + `AGENTS.md`, a `docs/` folder, `.cursorrules` —
any layout passes as long as an agent starting cold finds what it needs.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | A context file exists in the repo and describes what the project is, how it's structured, and anything surprising about it | S |
| 2 · Shared | One canonical file; any other agent-instruction files reference or include it rather than duplicating. It is current — an agent following it does not hit contradicted instructions | S |
| 3 · Critical | Domain vocabulary defined: the concepts have short shared names used consistently in code, docs and conversation. Architecture decisions are linked from it so agents find them | M |
| 4 · Contracted | As level 3 | — |

## Agent configuration reviewed as code

Agent instructions, skills and hooks live in the repository and go through the same review as
application code.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | Agent config is committed, not held in personal settings or a chat history | S |
| 2 · Shared | Changes to agent config go through the same PR review as code | S |
| 3 · Critical | As level 2 | — |
| 4 · Contracted | As level 2 | — |

## Destructive-action guardrails

There are boundaries on what agents may do, and at higher levels those boundaries are enforced by
machinery rather than by instructions an agent can talk itself out of.

The mechanism is tool-specific — a hook, a wrapper script, a permission profile, a sandboxed
environment. The facet files name the mechanism per stack.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Boundaries written down: what agents may and may not touch (e.g. no direct production access, no force pushes) | S |
| 3 · Critical | Boundaries enforced, not just documented — destructive commands (recursive deletes, force pushes, production deletes) are blocked by tooling before they run | M |
| 4 · Contracted | Enforcement covers production credentials and data as well as commands; agent actions against production are logged | M |

## Automated post-edit checks

Lint and format run automatically after an agent edits code, so mistakes are corrected before they
reach a commit.

This is the first of three enforcement points, and the cheapest. Pre-commit hooks are the second and
CI is authoritative — see *Pre-commit hooks* in Source Control & Review.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | Formatter and linter run automatically after agent edits, auto-fixing what they can | M |
| 3 · Critical | As level 2 | — |
| 4 · Contracted | As level 2 | — |

## Agent issue tracking

Agents know where work is tracked and how to read and file it.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | The tracker is named in the agent context file, with enough detail for an agent to find and read issues | S |
| 3 · Critical | A triage vocabulary exists and is documented — labels distinguishing what needs information, what is ready for an agent, and what needs a human | M |
| 4 · Contracted | As level 3 | — |

## Automated diagnosis skills

The repository ships skills that know the codebase and its environment well enough to diagnose
failures — a failed deployment, an unhealthy service — without a human reconstructing context first.

**Example.** A deployment-diagnosis skill knows: where the pipeline logs live and how to fetch the
failing run; which cloud resources the app depends on and how to query their health; the last known-
good release and how to compare against it; and the three failure modes this project actually has
(migration lock, expired credential, exhausted quota). It ends with a stated cause and a recommended
action, not a log dump.

| Level | Definition | Effort |
|---|---|---|
| 1 · Personal | — | |
| 2 · Shared | A written diagnosis runbook exists for the common failure modes | M |
| 3 · Critical | A skill in the repo reads deploy state and logs and reports a diagnosis; it names the environment and the resources rather than guessing | L |
| 4 · Contracted | The skill is wired into the incident path so it runs before a human is paged, and its output is attached to the incident record | L |
