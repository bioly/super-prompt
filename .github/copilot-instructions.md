# GitHub Copilot Instructions

Full rules and context for this repository live in `ai/core/`.
For Copilot Chat, reference files directly: `@workspace #file:ai/core/agents.md`

The rules below are a summary. The canonical source is `ai/core/`.

---

## Behaviour

- Read relevant files before proposing changes. Never suggest edits to code you have not seen.
- Keep changes scoped to what was asked. Do not refactor surrounding code.
- State assumptions explicitly. Ask one focused question rather than proceeding on a wrong assumption.
- New features ship with tests. Bug fixes ship with regression tests.
- Do not add comments, docstrings, or type annotations to code you did not change.

## Hard Constraints

- Never commit secrets, credentials, tokens, or environment-specific configuration.
- Never bypass authentication, authorisation, or input validation.
- Never use raw SQL string concatenation — use parameterised queries or ORM.
- Never take destructive or irreversible actions without explicit confirmation.
- Never add a dependency without checking whether the codebase already provides the capability.

## Code Quality

- One function, one responsibility. If you need "and" to describe it, split it.
- No dead code, unused imports, or commented-out blocks.
- Errors must surface loudly. Do not silently swallow exceptions.
- Names communicate intent. No abbreviations. Domain terms match the project's ubiquitous language.
- Maximum nesting: 3 levels. Invert conditions and return early instead.

## Architecture

- Respect existing module boundaries. Do not import across layers.
- Business logic lives in the service layer, not in controllers or handlers.
- Database access is isolated behind repositories. No raw queries in services.
- Configuration is injected, not imported directly from the environment.

## Testing

- Unit tests: no I/O, no network, no file system.
- Integration tests: real database, not mocked.
- Do not mock your own services — test through real implementations.
- Use Arrange / Act / Assert. One assertion per test where possible.

## Security

- Validate all external input at the boundary.
- Escape all user-supplied content before rendering.
- Least privilege: request only the permissions needed.
- Flag potential security issues proactively, even when not asked.

---

## Full Specification

For the complete rules, read:
- `ai/core/agents.md` — root spec and AI behaviour
- `ai/core/architecture.md` — structural constraints
- `ai/core/coding-standards.md` — style and naming
- `ai/core/testing.md` — testing requirements
- `ai/core/security.md` — security rules
- `ai/core/product.md` — project context and domain

For task-specific prompts:
- `ai/prompts/review.md`, `refactor.md`, `test-generator.md`, `bugfix.md`, `feature.md`
