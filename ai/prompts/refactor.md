# Prompt: Refactor

Use this prompt to refactor existing code for clarity, structure, or performance.

---

## Instructions for the AI

You are refactoring the specified code. Follow these rules strictly:

1. **Do not change external behaviour.** The refactored code must produce identical outputs for identical inputs.
2. **Do not expand scope.** Only refactor what is specified. Do not clean up surrounding code unless asked.
3. **Preserve the existing test suite.** All existing tests must still pass after the refactor.
4. **State your intent before changing.** Briefly describe what structural change you are making and why.

### What to look for

- Functions doing more than one thing → split them
- Deeply nested logic → invert conditions, extract helpers, return early
- Duplicated logic → extract a shared function or constant
- Unclear names → rename to communicate intent
- Long parameter lists → introduce an options object
- Implicit behaviour → make it explicit

### What to avoid

- Do not change the public interface unless asked
- Do not upgrade or change dependencies
- Do not add logging, metrics, or instrumentation
- Do not add error handling that did not exist before (unless a clear bug)
- Do not optimise prematurely — only address performance if it is the stated goal

---

## Usage

```
Refactor [file or function name].

Goal: [clarity / reduce duplication / improve performance / split responsibilities]

Constraints: [any constraints specific to this refactor]
```

### Example

```
Refactor the `processOrder` function in src/orders/order-service.ts.

Goal: It currently does too many things — split validation, enrichment, and persistence into separate functions.

Constraints: Do not change the function signature. Existing unit tests must still pass.
```

---

## Output Format

1. Brief description of the structural changes you are making.
2. The refactored code.
3. Any follow-up refactors you would recommend (but did not apply) — one sentence each.
