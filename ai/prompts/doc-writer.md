# Prompt: Documentation Writer

Use this prompt to generate or improve documentation for code, APIs, or architectural decisions.

---

## Instructions for the AI

You are writing documentation. The goal is to give the reader exactly what they need — no more.

### Rules

1. **Write for the intended audience.** A public API doc is for consumers; an internal ADR is for the team.
2. **Describe what, why, and when.** Not how — the code shows how.
3. **Show, don't just tell.** Code examples are more useful than prose descriptions of code.
4. **Keep it honest.** Do not document behaviour the code does not have. Do not omit known limitations.
5. **Do not pad.** Every sentence earns its place. Cut anything that does not add information.

---

## Documentation Types

### API / Function Documentation
- What it does (one sentence)
- Parameters: name, type, description, whether required, default value
- Return value: type and description
- Throws / rejects: error conditions
- Example: one or two real usage examples

### README
- What the project is and who it is for (one paragraph)
- Prerequisites
- Installation
- Quick start (working example in under 5 minutes)
- Configuration reference
- Link to further docs

### Architecture Decision Record (ADR)
- **Title:** Short noun phrase
- **Status:** Proposed / Accepted / Deprecated / Superseded
- **Context:** The situation that required a decision
- **Decision:** What was decided
- **Consequences:** Trade-offs, implications, follow-up work

### Inline Comments
- Only where the logic is non-obvious
- Explain *why*, not *what*
- Reference ticket/issue numbers for workarounds

---

## Usage

```
Write [type of documentation] for [target].

Audience: [e.g. external API consumers / internal team / onboarding developers]
Format: [e.g. JSDoc / Markdown / ADR]
Tone: [e.g. formal / conversational]
```

### Example

```
Write API documentation for the `POST /payments/charge` endpoint.

Audience: External developers integrating our payment API.
Format: OpenAPI-compatible Markdown.
Tone: Clear and professional.

[paste the relevant handler or schema]
```

---

## Output Format

Produce documentation in the requested format. Then:
- Note any gaps: missing information you could not derive from the code (e.g. undocumented error codes).
- Flag any inconsistencies between the code and existing documentation.
