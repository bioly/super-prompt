# AI Agent Specification

This is the root specification for all AI coding assistants in this repository.
Every tool adapter references this file first. Rules here apply everywhere.

---

## Philosophy

- **Minimum viable complexity** — The right amount of code is the least that solves the problem correctly.
- **Explicit over implicit** — Clarity beats cleverness. Name things for what they are.
- **Correctness before speed** — Do not ship broken code faster.
- **Security by default** — All external input is untrusted until validated.
- **Long-term ownership** — Write code as if you will maintain it in two years.

---

## Behaviour

### Before starting a task
- Read the relevant files before proposing changes. Never suggest edits to code you have not read.
- State assumptions explicitly when context is incomplete.
- Ask one focused clarifying question rather than proceeding on a wrong assumption.

### During a task
- Keep changes minimal and scoped to what was asked. Do not refactor surrounding code unless asked.
- Prefer editing existing files over creating new ones.
- Do not add comments, docstrings, or type annotations to code you did not change.
- Do not add error handling for scenarios that cannot happen.
- Do not add abstractions, helpers, or utilities for one-off operations.

### After completing a task
- Do not summarise what you just did — the diff is self-evident.
- If a decision was non-obvious, add a brief inline comment at the decision point. Not an essay.

---

## Constraints

- Never introduce breaking changes without an explicit instruction to do so.
- Never commit secrets, credentials, tokens, or environment-specific values.
- Never bypass security controls (authentication, authorisation, input validation, sanitisation).
- Never take destructive or irreversible actions (drop tables, delete files, force-push) without explicit confirmation.
- Never add a dependency without first checking whether the codebase already provides the capability.
- Never generate or guess URLs unless they are needed for code functionality.

---

## Code Quality

- All new code must be testable and tested.
- No dead code, unused imports, or commented-out blocks in committed code.
- Errors must surface loudly — do not silently swallow exceptions.
- Validate at system boundaries (user input, external APIs). Trust internal code and framework guarantees.
- Names must communicate intent. Avoid abbreviations unless they are universally understood in this domain.

---

## Safety

- Flag potential security issues even when not asked.
- Flag irreversible operations before executing them.
- Do not proceed on ambiguous destructive instructions — ask first.
- When generating infrastructure or configuration, default to least privilege.

---

## References

Full rules are split across focused files. Read them in this order when context allows:

1. [product.md](product.md) — Project context, domain, goals
2. [architecture.md](architecture.md) — Structural constraints and module boundaries
3. [coding-standards.md](coding-standards.md) — Style, naming, formatting
4. [testing.md](testing.md) — Testing philosophy and requirements
5. [security.md](security.md) — Security rules and threat model
6. [ui-ux.md](ui-ux.md) — UI/UX guidelines

Reusable task prompts: [../prompts/](../prompts/)
Claude-specific schemas: [../schema/](../schema/)
