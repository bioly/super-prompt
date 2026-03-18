# Agent Schema

How to define an AI agent for use with Claude Code (and conceptually with any tool that supports autonomous agents).

---

## What is an Agent?

An agent is an autonomous AI unit with a defined purpose, a bounded context, a set of permitted tools, and a set of commands it can execute. Agents are scoped — they do not have unlimited access to the system.

---

## Agent Definition Structure

```markdown
# Agent: [Name]

## Purpose
One paragraph: what this agent does, when to invoke it, and what problem it solves.

## Context Files
The files the agent reads to understand its operating environment:
- path/to/relevant-spec.md
- path/to/relevant-rules.md

## Scope
What the agent is allowed to work on (files, modules, systems).
What is explicitly out of scope.

## Permitted Tools
List the tools this agent may use:
- Read — read files
- Write / Edit — write or modify files (specify if limited to certain paths)
- Bash — run shell commands (specify allowed commands explicitly)
- WebSearch / WebFetch — if the agent needs external information
- [Other tools as appropriate]

## Commands
The slash commands this agent exposes:
- /command-name — what it does

## Behaviour
How the agent should operate:
- Tone, level of autonomy, when to ask vs act
- What to do when blocked or uncertain
- Output format

## Constraints
Hard limits — what this agent must never do.
```

---

## Example: Code Review Agent

```markdown
# Agent: reviewer

## Purpose
Reviews pull request diffs for correctness, security, architecture, and code quality.
Invoke this agent with a diff or list of changed files to get structured review feedback.

## Context Files
- ai/core/agents.md
- ai/core/architecture.md
- ai/core/coding-standards.md
- ai/core/security.md
- ai/core/testing.md

## Scope
In scope: any source file in the diff.
Out of scope: CI configuration, lockfiles, generated files.

## Permitted Tools
- Read — to read source files referenced in the diff
- Glob — to find related files for context

## Commands
- /review — run a full review on the current diff

## Behaviour
- Group findings by severity: Blocker, Suggestion, Nit.
- Reference exact file paths and line numbers.
- Be direct. No padding or praise.
- If the diff is too large to review meaningfully, ask for a scoped subset.

## Constraints
- Do not modify any files.
- Do not run tests or build commands.
- Do not approve or reject — produce findings only.
```

---

## Agent Design Guidelines

- **One purpose per agent.** An agent that does everything is an agent that does nothing well.
- **Minimal permissions.** Only grant tools the agent actually needs. No `Bash` for read-only agents.
- **Explicit scope.** Define what is out of scope as clearly as what is in scope.
- **Idempotent where possible.** Running an agent twice should not produce worse results than running it once.
- **Fail explicitly.** Agents should state clearly when they cannot complete a task, rather than silently producing partial output.

---

## Conceptual Portability

While the syntax above uses Claude Code conventions, the concepts map to other tools:
- **Context files** → any tool's equivalent of "system context" or "background files"
- **Permitted tools** → any tool's capability restrictions
- **Commands** → task types or invocation modes
- **Constraints** → guardrails enforced by the tool or by instruction
