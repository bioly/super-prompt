# Claude Code

This file is the Claude Code entrypoint for this repository.
All rules and context live in `ai/core/`. This file only loads them.

## Context

Read and apply the following files as your context, in this order:

@ai/core/agents.md
@ai/core/product.md
@ai/core/architecture.md
@ai/core/coding-standards.md
@ai/core/testing.md
@ai/core/security.md
@ai/core/ui-ux.md

## Claude-Specific Schemas

When asked to create an agent, command, skill, or multi-agent flow, follow the schemas in:

@ai/schema/agent-schema.md
@ai/schema/command-schema.md
@ai/schema/skill-schema.md
@ai/schema/multi-agent-patterns.md

## Prompts

Reusable task prompts are in `ai/prompts/`. When the user asks for a review, refactor, test generation, etc., load and apply the relevant prompt:

- Review → @ai/prompts/review.md
- Refactor → @ai/prompts/refactor.md
- Generate tests → @ai/prompts/test-generator.md
- Write docs → @ai/prompts/doc-writer.md
- Design API → @ai/prompts/api-designer.md
- Fix bug → @ai/prompts/bugfix.md
- Implement feature → @ai/prompts/feature.md
