# Agent Instructions

This file is the entrypoint for OpenAI Codex and any agent that reads `AGENTS.md`.
All rules and context live in `ai/core/`. This file only loads them.

## Instructions

Read and apply the following files as your context before starting any task.
Read them in order — later files may add to or refine earlier ones.

1. `ai/core/agents.md` — Global philosophy, constraints, and how to behave
2. `ai/core/product.md` — Project context, domain, and tech stack
3. `ai/core/architecture.md` — Structural constraints and module boundaries
4. `ai/core/coding-standards.md` — Style, naming, and formatting rules
5. `ai/core/testing.md` — Testing requirements and philosophy
6. `ai/core/security.md` — Security rules and non-negotiables
7. `ai/core/ui-ux.md` — UI/UX guidelines (skip if working on non-UI code)

## Prompts

When asked to perform a specific type of task, read the corresponding prompt file first:

- Code review → `ai/prompts/review.md`
- Refactoring → `ai/prompts/refactor.md`
- Writing tests → `ai/prompts/test-generator.md`
- Documentation → `ai/prompts/doc-writer.md`
- API design → `ai/prompts/api-designer.md`
- Bug investigation → `ai/prompts/bugfix.md`
- Feature implementation → `ai/prompts/feature.md`

## Behaviour Summary

The full rules are in the files above. The most important:

- Read before writing. Never suggest changes to code you have not read.
- Ask one clarifying question rather than proceeding on a wrong assumption.
- Keep changes scoped. Do not refactor what you were not asked to refactor.
- New features ship with tests.
- Never commit secrets, credentials, or environment-specific values.
- Flag security issues even when not asked.
