# Prompt: Test Generator

Use this prompt to generate tests for existing code.

---

## Instructions for the AI

You are writing tests for the specified code. Follow the conventions in `testing.md`.

### Rules

1. **Read the code first.** Understand what it does before writing tests. Do not guess behaviour.
2. **Tests must be able to fail.** Do not write tests that always pass regardless of the implementation.
3. **Cover meaningful cases, not just lines.** Coverage is a floor, not a goal.
4. **Use real dependencies where possible.** Mock only at the outermost system boundary (HTTP, email, etc.).
5. **Each test is independent.** No shared state between tests.

### Required cases to cover

For every function or module:
- **Happy path** — correct input, expected output
- **Edge cases** — empty inputs, boundary values, nulls/undefined, empty collections
- **Error cases** — invalid input, unavailable dependencies, expected failure modes

For functions with side effects:
- **Verify the side effect occurred** — not just that no error was thrown

For async code:
- **Verify rejection handling** — test what happens when promises reject

---

## Usage

```
Generate tests for [file or function name].

Type: [unit / integration / e2e]
Framework: [e.g. Vitest, Jest, pytest, RSpec — leave blank to infer from project]
Focus: [optional — e.g. "focus on error cases" or "the happy path already has tests"]
```

### Example

```
Generate tests for the `calculateDiscount` function in src/pricing/discount-calculator.ts.

Type: unit
Framework: Vitest

Focus: Edge cases — zero quantities, negative values, expired promotion codes.
```

---

## Output Format

For each test case, include:
1. A descriptive test name (behaviour, not implementation)
2. The test body using Arrange / Act / Assert
3. A one-line comment if the case is non-obvious

After the tests, list any cases you chose **not** to test and why (e.g. "Skipped testing the logger call — it is a side effect with no impact on correctness here").
