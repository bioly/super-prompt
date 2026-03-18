# Skill Schema

How to define reusable skills — packaged capabilities that agents and commands can invoke.

---

## What is a Skill?

A skill is a self-contained, reusable capability. Where a command is a user-facing action, a skill is a building block that multiple commands or agents can use internally.

Think of skills as functions: they take inputs, perform a well-defined operation, and produce outputs. They can be composed.

---

## Skill Definition Structure

```markdown
# Skill: [name]

## Purpose
One sentence: what capability this skill provides.

## Inputs
| Input | Type | Required | Description |
|---|---|---|---|
| target | string | Yes | What to operate on |
| options | object | No | Configuration options |

## Outputs
| Output | Type | Description |
|---|---|---|
| result | string | The produced content or report |
| metadata | object | Optional metadata about the operation |

## Steps
Ordered description of what the skill does:
1. Step one
2. Step two
3. Step three

## Error Conditions
When this skill should stop and return an error:
- Condition → error message / fallback behaviour

## Used by
Commands and agents that invoke this skill:
- /command-name
- agent-name
```

---

## Example: Diff Analysis Skill

```markdown
# Skill: analyse-diff

## Purpose
Parse a git diff and extract structured information about what changed.

## Inputs
| Input | Type | Required | Description |
|---|---|---|---|
| diff | string | Yes | Raw git diff output |
| focus | string[] | No | Limit analysis to specific file patterns |

## Outputs
| Output | Type | Description |
|---|---|---|
| files_changed | string[] | List of affected file paths |
| additions | number | Total lines added |
| deletions | number | Total lines deleted |
| summary | string | Human-readable summary of changes |
| risk_areas | string[] | Files/areas that warrant extra attention |

## Steps
1. Parse the diff into file-level sections.
2. Identify the change type per file: new, modified, deleted, renamed.
3. Flag high-risk areas: auth, payments, DB migrations, public API surface.
4. Produce a structured summary.

## Error Conditions
- Empty diff → return early with "No changes detected"
- Unparseable diff format → return error with raw input for debugging

## Used by
- /review
- reviewer agent
```

---

## Example: Context Loader Skill

```markdown
# Skill: load-context

## Purpose
Load and merge the relevant core spec files for the current task.

## Inputs
| Input | Type | Required | Description |
|---|---|---|---|
| task_type | string | Yes | The type of task: review, feature, bugfix, refactor |

## Outputs
| Output | Type | Description |
|---|---|---|
| context | string | Merged content of relevant spec files |
| files_loaded | string[] | Which files were included |

## Steps
1. Map task_type to the relevant spec files:
   - review → agents.md, architecture.md, coding-standards.md, security.md, testing.md
   - feature → agents.md, product.md, architecture.md, coding-standards.md, testing.md
   - bugfix → agents.md, architecture.md, coding-standards.md
   - refactor → agents.md, architecture.md, coding-standards.md
2. Read each file.
3. Return merged content.

## Error Conditions
- Unknown task_type → load all core files as fallback

## Used by
- All commands (via their respective agents)
```

---

## Skill Design Guidelines

- **Pure where possible.** Skills that do not have side effects are easier to test and compose.
- **Single responsibility.** A skill does one thing. Compose skills to do more.
- **Explicit inputs and outputs.** No implicit state. Everything passed in, everything returned.
- **Idempotent.** Running a skill twice with the same inputs produces the same output.
- **Self-describing errors.** When a skill cannot run, say exactly why.
