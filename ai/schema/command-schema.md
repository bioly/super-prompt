# Command Schema

How to define slash commands for use in AI coding tools.

---

## What is a Command?

A command is a named, invocable action that triggers a specific AI behaviour. Commands give teams a shared vocabulary for common tasks, making them repeatable, reviewable, and consistent across sessions.

---

## Command Definition Structure

```markdown
# Command: /[name]

## Purpose
One sentence: what this command does.

## Invocation
How the user calls this command, including optional arguments:
/name [required-arg] [--optional-flag value]

## Arguments
| Argument | Required | Description |
|---|---|---|
| target | Yes | The file, function, or scope to operate on |
| --flag | No | What this flag does (default: value) |

## Behaviour
Step-by-step description of what the command does when invoked:
1. Read the specified target
2. Apply the relevant prompt from ai/prompts/
3. Output results in the defined format

## Output Format
Description of what the command produces.

## Example
/name src/orders/order-service.ts --flag value
```

---

## Example: Review Command

```markdown
# Command: /review

## Purpose
Run a structured code review on a file or diff using the review prompt.

## Invocation
/review [file-or-diff] [--focus area]

## Arguments
| Argument | Required | Description |
|---|---|---|
| file-or-diff | Yes | File path, git diff, or "staged" for staged changes |
| --focus | No | Narrow the review: security, tests, architecture, all (default: all) |

## Behaviour
1. Read the specified file or diff.
2. Load context from ai/core/agents.md, architecture.md, security.md.
3. Apply the review prompt from ai/prompts/review.md.
4. Output structured findings grouped by severity.

## Output Format
Blockers → Suggestions → Nits, each with file:line references.

## Example
/review src/auth/auth.service.ts --focus security
/review staged
```

---

## Example: Feature Command

```markdown
# Command: /feature

## Purpose
Scaffold and implement a new feature following the feature prompt.

## Invocation
/feature "[description]" [--plan-only]

## Arguments
| Argument | Required | Description |
|---|---|---|
| description | Yes | Natural language description of the feature |
| --plan-only | No | Output only the implementation plan, no code |

## Behaviour
1. Read relevant existing code to understand the pattern to follow.
2. Produce an implementation plan (pause for review on non-trivial features).
3. If --plan-only is not set, implement the plan.
4. Generate tests.

## Output Format
Plan → Implementation → Tests → Follow-up notes.

## Example
/feature "allow users to export their profile as a PDF" --plan-only
```

---

## Command Design Guidelines

- **Single responsibility.** Each command does one thing.
- **Predictable output.** The same inputs produce equivalent outputs. No surprises.
- **Reference prompts, not duplicate them.** Commands orchestrate; prompts contain logic.
- **Composable.** Design commands so they can be chained: `/review` then `/fix`.
- **Fail clearly.** If a command cannot run (missing argument, ambiguous target), it says so and stops.

---

## Command Registry

Maintain a list of all active commands here:

| Command | Description | Prompt used |
|---|---|---|
| /review | Code review | ai/prompts/review.md |
| /refactor | Refactor code | ai/prompts/refactor.md |
| /test | Generate tests | ai/prompts/test-generator.md |
| /bugfix | Investigate and fix a bug | ai/prompts/bugfix.md |
| /feature | Implement a new feature | ai/prompts/feature.md |
| /docs | Write documentation | ai/prompts/doc-writer.md |
| /api | Design an API contract | ai/prompts/api-designer.md |

> Add custom commands specific to your project below.
