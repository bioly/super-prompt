# Architecture

Structural constraints, module boundaries, and patterns that must be respected across all changes.

---

## Structure Rules

- Respect existing module and package boundaries. Do not import across layers unless the current architecture already does.
- Business logic lives in the domain/service layer, not in controllers, resolvers, or route handlers.
- Controllers/handlers are thin: validate input, call service, return response. No logic.
- Database access is isolated behind a repository or data-access layer. Services do not write raw queries.
- Configuration is injected, not imported directly. No `process.env` calls deep in domain code.

---

## Dependency Direction

```
Presentation (UI / API handlers)
        ↓
Application (use cases / services)
        ↓
Domain (entities, rules, interfaces)
        ↓
Infrastructure (DB, external APIs, file system)
```

- Upper layers may depend on lower layers. Never the reverse.
- Cross-cutting concerns (logging, auth, tracing) use middleware or decorators, not direct calls.

---

## Module Boundaries

> Customise this section for your project's actual modules.

- Each module owns its own data. Do not reach into another module's database tables or internal state.
- Shared types and interfaces live in a `shared/` or `common/` package. Do not duplicate them.
- Public module APIs are declared explicitly. Internal implementation is not importable from outside.

---

## Patterns in Use

> Document which patterns are established in this codebase. The AI will follow them.

- **Repository pattern** — all DB access through repository interfaces
- **Service layer** — business logic in service classes/functions
- <!-- Add: Event sourcing, CQRS, Saga, etc. if applicable -->

---

## Patterns to Avoid

- **God classes / god files** — If a file exceeds ~300 lines, it should be split.
- **Anemic domain model** — Domain objects should have behaviour, not just be data bags.
- **Callback hell / promise chains** — Use async/await.
- **Global mutable state** — Avoid module-level mutable singletons.
- **Implicit coupling** — If two modules need to communicate, make the interface explicit.

---

## File and Folder Conventions

> Customise for your project's actual structure.

```
src/
  domain/          # Entities, value objects, domain rules
  application/     # Use cases, service orchestration
  infrastructure/  # DB, external APIs, file system adapters
  presentation/    # HTTP handlers, GraphQL resolvers, CLI commands
  shared/          # Types, utils, constants used across layers
tests/
  unit/            # Fast, isolated, no I/O
  integration/     # Real DB, real services, controlled environment
  e2e/             # Full stack, against a running instance
```

---

## When Proposing Structural Changes

- Do not restructure the project unless asked.
- If a natural refactor would significantly improve correctness or safety, flag it as a suggestion — do not do it silently.
- New modules follow the same structure as existing modules. Check existing modules before scaffolding.
