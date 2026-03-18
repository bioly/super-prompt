# Prompt: Code Review

Use this prompt to get a structured code review of a file, diff, or pull request.

---

## Instructions for the AI

You are performing a code review. Be direct and specific. Reference exact line numbers or code snippets.
Group feedback by severity. Do not pad with praise — the developer wants signal, not noise.

### Review dimensions (in priority order)

1. **Correctness** — Does the code do what it is supposed to do? Are there logic errors, off-by-one errors, race conditions, unhandled edge cases?
2. **Security** — Does it follow the rules in `security.md`? SQL injection, XSS, missing auth checks, secrets in code, etc.
3. **Architecture** — Does it respect module boundaries and the patterns in `architecture.md`?
4. **Testing** — Are the tests meaningful? Do they cover the right cases? Can they actually fail?
5. **Code quality** — Clarity, naming, duplication, complexity. Apply `coding-standards.md`.
6. **Performance** — Only flag genuine concerns (N+1 queries, unbounded loops on large datasets, missing indexes). Do not speculate.

### Severity levels

- **Blocker** — Must be fixed before merge. Correctness, security, or architectural violations.
- **Suggestion** — Should be addressed but is not blocking. Quality, clarity, test coverage.
- **Nit** — Minor style or naming issues. Batched at the end. Never blocking.

---

## Usage

```
Review [file, diff, or PR description].

Context: [what this change is meant to do]
Focus: [optional — e.g. "focus on security" or "skip nits"]
```

### Example

```
Review the diff below.

Context: This adds a file upload endpoint that accepts user-supplied images and stores them in S3.

Focus: Security and error handling.

[paste diff]
```

---

## Output Format

### Summary
One paragraph: overall assessment, biggest concerns, and whether you would approve as-is.

### Blockers
- [file:line] — Description of the issue and required fix.

### Suggestions
- [file:line] — Description and recommended change.

### Nits
- [file:line] — Minor issues, batched. No action required.
