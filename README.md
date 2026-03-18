# AI Development Infrastructure

Single source of truth for rules, coding standards, and prompts across all AI coding tools.

## Philosophy

- **No duplication** — Edit once, all tools update.
- **No tool-specific content in core** — Core files are pure Markdown, tool-agnostic.
- **Thin adapters** — Each tool file is only a loader; no logic lives there.
- **Separation of concerns** — Rules live in `core/`, reusable prompts in `prompts/`, tool schemas in `schema/`.

---

## Folder Structure

```
README.md                    # This file
CLAUDE.md                    # Claude Code entrypoint (root)
AGENTS.md                    # Codex / OpenAI entrypoint (root)
.github/
  copilot-instructions.md    # GitHub Copilot entrypoint
ai/
  core/
    agents.md                # Root spec: philosophy, constraints, AI behaviour
    product.md               # Project context, domain, goals (fill this in)
    architecture.md          # Architectural patterns and constraints
    coding-standards.md      # Style, naming, conventions
    testing.md               # Testing philosophy and requirements
    security.md              # Security rules and threat model
    ui-ux.md                 # UI/UX guidelines
  prompts/
    refactor.md              # Prompt: refactor existing code
    review.md                # Prompt: code review
    test-generator.md        # Prompt: generate tests
    doc-writer.md            # Prompt: write documentation
    api-designer.md          # Prompt: design API contracts
    bugfix.md                # Prompt: investigate and fix bugs
    feature.md               # Prompt: implement a new feature
  schema/
    agent-schema.md          # How to define an AI agent
    command-schema.md        # How to declare slash commands
    skill-schema.md          # How to model reusable skills
    multi-agent-patterns.md  # Patterns for multi-agent orchestration
```

---

## How Each Tool Loads the Core Files

### Claude Code (`CLAUDE.md`)

Claude Code supports `@file` includes. The root `CLAUDE.md` uses this to load core files in order:

```markdown
@ai/core/agents.md
@ai/core/product.md
@ai/core/architecture.md
...
```

**To use:** Place `CLAUDE.md` at the repository root. Claude Code loads it automatically.

### GitHub Copilot (`.github/copilot-instructions.md`)

Copilot loads this file as a system prompt — no native include mechanism exists. The adapter contains the most critical rules inline and references the full spec for human readers.

**To use:** The `.github/copilot-instructions.md` file is picked up automatically by GitHub Copilot in VS Code and JetBrains IDEs.
For full context in Copilot Chat, reference files explicitly: `@workspace #file:ai/core/agents.md`.

### Codex (`AGENTS.md`)

Codex reads `AGENTS.md` at the repository root (and subdirectory `AGENTS.md` files for scoped instructions). It follows instructions to read additional files.

**To use:** Place `AGENTS.md` at the repository root. Codex picks it up automatically when run from that directory.

### Adding a New Tool

1. Create an adapter file at the tool's expected location.
2. Make it a loader only: list the core files to read, in order.
3. Add any tool-specific configuration that has no equivalent in core.
4. Document it here.

---

## Customising the Core

| What you want to change | File to edit |
|---|---|
| Project goals, tech stack, domain | `ai/core/product.md` |
| Code style, naming rules | `ai/core/coding-standards.md` |
| Folder structure, module boundaries | `ai/core/architecture.md` |
| Test coverage requirements | `ai/core/testing.md` |
| Security constraints | `ai/core/security.md` |
| AI behaviour, constraints, persona | `ai/core/agents.md` |
| Reusable task prompts | `ai/prompts/*.md` |

**Rule:** Never edit adapter files to add rules. If a rule belongs to all tools, it belongs in `core/`.

---

## Update Workflow

```
Edit ai/core/ or ai/prompts/
        ↓
All adapters automatically reflect changes
        ↓
Commit to version control
        ↓
Team pulls → all tools updated
```

No synchronisation step. No build process. Edit markdown, done.

---

## Onboarding a New Developer

1. Clone the repository.
2. Install their preferred AI tool (Claude Code, Copilot, Codex).
3. The adapter file is already in place — no setup required.
4. Read `ai/core/agents.md` to understand team conventions.

---

## Design Decisions

Key decisions behind this structure and why they were made this way.

| Decision | Why |
|---|---|
| `core/` as the source of truth (not `rules/`) | `core/` signals canonical, not suggestions |
| Adapter files at each tool's canonical location (root / `.github/`) | Each tool looks in exactly one place — no symlinks or extra indirection |
| No `ai/adapters/` subfolder | Having both `ai/adapters/CLAUDE.md` and root `CLAUDE.md` adds confusion; canonical location is enough |
| Copilot adapter inlines critical rules | Copilot has no `@include` — inlining the essentials means it works without manual file references in Chat |
| `product.md` is a fill-in template | The one file that is genuinely project-specific; all others ship with sensible defaults |
| `schema/` uses Claude concepts but written conceptually | The concepts (agents, commands, skills) map to every tool; only the syntax is Claude-native |
| Prompts are tool-agnostic instruction files | Any tool can be told "apply the rules in `ai/prompts/review.md`" — no tool-specific prompt copies |
| `agents.md` is the root spec, not a tool config | It defines how AI should behave in this repo — referenced by every adapter equally |

---

## Versioning and Testing

- Tag releases when core rules change significantly (e.g. `ai-rules/v1.2`).
- Review `ai/core/` changes in PRs like any other code — rules are code.
- Use `ai/prompts/review.md` to have the AI validate its own instruction changes.
