# Prompt: Feature Implementation

Use this prompt to implement a new feature.

---

## Instructions for the AI

You are implementing a feature. Work methodically: understand the goal, explore existing code, plan, then implement.

### Process

1. **Read before writing.** Understand the relevant existing code before proposing or writing anything.
2. **State your plan.** Before generating code, describe the approach in 3–5 bullet points. Wait for confirmation if the plan is non-trivial.
3. **Follow existing patterns.** Find an analogous feature in the codebase and follow the same structure.
4. **Implement incrementally.** Core logic first, then edge cases, then tests.
5. **Tests are part of the feature.** The implementation is not done until tests are written.

### Rules

- Do not add functionality beyond what is specified. Implement exactly what was asked.
- Do not add configuration flags, feature flags, or extensibility hooks unless asked.
- Do not add logging, metrics, or instrumentation unless asked.
- Do not refactor existing code as part of implementing a feature — raise it as a separate suggestion.
- Prefer editing existing files over creating new ones.

---

## Usage

```
Implement: [feature description]

Acceptance criteria:
- [criterion 1]
- [criterion 2]

Constraints: [any constraints — performance, backwards compatibility, etc.]
Out of scope: [explicitly what this feature does NOT include]
```

### Example

```
Implement: Allow users to export their data as a CSV file.

Acceptance criteria:
- Authenticated users can trigger a CSV export via POST /users/me/export
- The export includes: name, email, created_at, last_login
- Large exports (>10k rows) are generated asynchronously and emailed when ready
- Small exports (<= 10k rows) are returned as a file download immediately

Constraints: Must not block the main request thread for large exports.
Out of scope: Excel format, partial exports, admin-triggered exports.
```

---

## Output Format

1. **Plan** — How you will implement the feature (bullet points). Pause here for complex features.
2. **Implementation** — The code changes, in logical order (types → service → handler → tests).
3. **Tests** — Unit and/or integration tests covering the acceptance criteria.
4. **Follow-up** — Anything deferred, any risks, any related work this unblocks.
