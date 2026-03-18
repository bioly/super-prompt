# Coding Standards

Style, naming, formatting, and conventions applied to all code in this repository.

---

## General

- Prefer readability over brevity. A longer, clear name beats a short, cryptic one.
- Delete unused code. Do not comment it out.
- One responsibility per function. If you need "and" to describe what a function does, split it.
- Functions should be short enough to read in one screen. If they are not, extract helpers.
- Do not nest more than 3 levels deep. Invert conditions and return early instead.

---

## Naming

| Construct | Convention | Example |
|---|---|---|
| Variables, functions | camelCase | `getUserById`, `isActive` |
| Classes, types, interfaces | PascalCase | `UserService`, `PaymentResult` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Files (modules) | kebab-case | `user-service.ts`, `payment-utils.ts` |
| Test files | `*.test.ts` or `*.spec.ts` | `user-service.test.ts` |
| Booleans | Verb prefix: `is`, `has`, `can`, `should` | `isLoading`, `hasPermission` |

> Adjust conventions to match your language and ecosystem (e.g. Python uses `snake_case` for functions).

- Avoid generic names: `data`, `info`, `manager`, `handler`, `util` without more context.
- Avoid abbreviations: `usr`, `cfg`, `res` — write `user`, `config`, `response`.
- Domain terms must match the ubiquitous language defined in `product.md` exactly.

---

## Functions and Methods

- Maximum function length: ~30 lines before extracting.
- Parameters: prefer named parameters / option objects when a function takes more than 2 arguments.
- Avoid side effects in functions named as queries (`getX`, `findX`, `calculateX`).
- Command functions (`createX`, `deleteX`, `sendX`) may have side effects; name them accordingly.

---

## Error Handling

- Use typed errors. Do not throw bare strings.
- Handle errors at the boundary where you have enough context to recover or report meaningfully.
- Do not catch errors just to rethrow them unchanged — let them propagate.
- Never swallow errors silently with an empty catch block.
- Distinguish between operational errors (expected, handle gracefully) and programmer errors (fail fast, crash loudly).

---

## Imports and Dependencies

- Group imports: external packages → internal modules → relative imports. Blank line between groups.
- Use absolute imports for cross-module references, relative imports for same-module files.
- No circular imports.
- Do not import internal implementation details from another module — use its public API only.

---

## Comments

- Write comments to explain *why*, not *what*. The code says what.
- A comment that restates the code in English is noise — delete it.
- TODO comments must include a ticket/issue reference: `// TODO(#123): remove after migration`
- Do not leave `console.log`, `print`, `debugger` statements in committed code.

---

## Formatting

- Formatting is enforced by the project's linter/formatter (Prettier, ESLint, Black, etc.).
- Do not reformat code you did not change — it pollutes diffs.
- If the formatter and this guide conflict, the formatter wins.

---

## Language-Specific Defaults

> Add a section per language used in this project. Remove unused sections.

### TypeScript / JavaScript
- Strict mode enabled. No `any` unless there is no alternative and it is documented.
- Use `const` by default, `let` only when reassignment is required. Never `var`.
- Use `undefined` for missing values; avoid `null` unless interfacing with a system that requires it.
- Async functions always return a `Promise`. Never mix callbacks and promises.

### Python
- Follow PEP 8. Format with Black.
- Type hints on all public functions.
- Use dataclasses or Pydantic models for structured data; avoid plain dicts as return types.
- Prefer `pathlib.Path` over `os.path`.

### Go
- Follow `gofmt` / `goimports`.
- Errors are returned, not thrown. Check every error.
- Prefer explicit over idiomatic when readability is at stake.
