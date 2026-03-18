# Testing

Testing philosophy, requirements, and conventions for this repository.

---

## Philosophy

- Tests are a first-class deliverable. New features ship with tests; bug fixes ship with a regression test.
- Tests document intent. A well-named test is the best specification of what code should do.
- Prefer real behaviour over mocks. Only mock at the outermost system boundary (external HTTP, email, payment gateway).
- Tests must be deterministic. Flaky tests are bugs — fix or delete them.
- A test that always passes is worthless. Ensure tests can fail.

---

## Test Pyramid

```
          /\
         /E2E\          Few — slow, expensive, cover critical paths only
        /------\
       /Integr. \       Moderate — real DB, real services, controlled environment
      /----------\
     /    Unit    \     Many — fast, isolated, one concern per test
    /--------------\
```

- Unit tests: no I/O, no network, no file system. Run in milliseconds.
- Integration tests: real database (not mocked), real queues, controlled external services.
- E2E tests: full stack against a running application. Cover user journeys, not implementation details.

---

## Coverage Requirements

> Customise these thresholds for your project.

- Line coverage target: **80%** minimum, **90%** for core domain logic.
- Coverage is a floor, not a goal. 100% coverage with bad tests is worse than 80% with good ones.
- Critical paths (payment, auth, data deletion) require explicit test cases for both happy path and failure modes.

---

## Test Structure

Use the **Arrange / Act / Assert** (AAA) pattern:

```typescript
it('returns null when user does not exist', async () => {
  // Arrange
  const repo = createUserRepository({ users: [] });

  // Act
  const result = await repo.findById('non-existent-id');

  // Assert
  expect(result).toBeNull();
});
```

- One assertion per test where possible. Multiple assertions are acceptable when they test one cohesive outcome.
- Test names describe behaviour, not implementation: `returns null when user does not exist`, not `test findById null`.
- Use `describe` blocks to group related tests by the unit under test and scenario.

---

## Naming Conventions

| Test type | File location | Naming pattern |
|---|---|---|
| Unit | Next to the source file | `user-service.test.ts` |
| Integration | `tests/integration/` | `user-repository.integration.test.ts` |
| E2E | `tests/e2e/` | `checkout-flow.e2e.test.ts` |

---

## What to Mock (and What Not To)

**Mock:**
- External HTTP APIs (Stripe, SendGrid, etc.) — use recorded fixtures or a test double
- Time (`Date.now()`, `new Date()`) — inject or mock for deterministic tests
- Random values — inject seeds or mock generators

**Do not mock:**
- The database — use a real test database, seeded per test
- Your own services — test through real service implementations
- The unit under test itself

---

## Test Data

- Each test is responsible for its own setup and teardown. Do not rely on shared state between tests.
- Use factory functions or builders to create test data, not raw object literals.
- Seed only the data the test actually needs. Minimal fixtures reduce coupling.

---

## Running Tests

> Update these commands to match the actual project setup.

```bash
# All tests
pnpm test

# Unit tests only (fast feedback loop)
pnpm test:unit

# Integration tests (requires running services)
pnpm test:integration

# Coverage report
pnpm test:coverage
```

---

## When Generating Tests

- Generate tests that can actually fail. Verify the failure mode.
- Cover: happy path, edge cases, and the most likely error conditions.
- Do not generate tests for framework internals or trivial getters/setters.
- If the code under test is hard to test, flag it as a design issue rather than working around it with complex mocking.
