# Prompt: Bug Fix

Use this prompt to investigate and fix a bug.

---

## Instructions for the AI

You are diagnosing and fixing a bug. Work systematically. Do not guess — trace the problem.

### Process

1. **Understand the expected behaviour.** What should happen?
2. **Understand the actual behaviour.** What is happening instead?
3. **Identify the root cause.** Read the relevant code. Trace data flow. State what is wrong and why.
4. **Fix the root cause, not the symptom.** Do not suppress errors or add workarounds that mask the real problem.
5. **Write a regression test.** The bug must have a test that would have caught it.
6. **Verify the fix does not break existing tests.**

### Rules

- Do not expand scope. Fix the reported bug. Do not refactor surrounding code.
- Do not change behaviour beyond what is needed to fix the bug.
- If the fix requires a larger structural change, say so and propose it — do not silently do it.
- If the root cause is unclear after reading the relevant code, state what additional information is needed.

---

## Usage

```
Fix: [description of the bug]

Expected behaviour: [what should happen]
Actual behaviour: [what is happening]
Reproduction steps: [how to trigger the bug]
Environment: [optional — OS, runtime version, etc.]

Relevant files: [optional — list known relevant files]
```

### Example

```
Fix: Users with special characters in their email address cannot reset their password.

Expected behaviour: Password reset email is sent successfully for any valid email address.
Actual behaviour: The endpoint returns 500 for emails containing + or . characters.
Reproduction steps:
  1. Register an account with email user+tag@example.com
  2. POST /auth/reset-password with that email
  3. 500 Internal Server Error is returned

Relevant files: src/auth/password-reset.service.ts, src/auth/auth.controller.ts
```

---

## Output Format

1. **Root cause** — What the bug is and exactly where in the code it occurs.
2. **Fix** — The corrected code.
3. **Regression test** — A test case that would have caught this bug.
4. **Risk** — Any related code that may have the same problem or that the fix could affect.
