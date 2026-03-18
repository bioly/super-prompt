# Security

Security rules, constraints, and threat model for this repository.

---

## Non-Negotiable Rules

These apply to every change, without exception:

1. **Never commit secrets** — no API keys, tokens, passwords, or private keys in source code or config files. Use environment variables and a secrets manager.
2. **Validate all input at the boundary** — treat every value from the network, file system, or user as untrusted.
3. **Never concatenate SQL** — use parameterised queries or an ORM. Always.
4. **Never render unsanitised user content as HTML** — escape or sanitise before rendering.
5. **Enforce authentication before authorisation** — never check what a user can do before confirming who they are.
6. **Apply least privilege** — IAM roles, DB users, API tokens should have only the permissions they need.
7. **Never log sensitive data** — passwords, tokens, PII, payment data must not appear in logs.

---

## Input Validation

- Validate schema (types, shapes) at the API boundary before business logic touches the data.
- Validate semantics (ranges, formats, allowed values) in the domain layer.
- Reject unknown fields rather than silently ignoring them.
- For file uploads: validate MIME type, extension, size, and scan for malicious content.

---

## Authentication and Authorisation

- Authentication is handled by the established auth layer. Do not implement custom auth unless explicitly asked.
- Authorisation checks must happen server-side. Never trust client-provided role or permission claims.
- Re-verify authorisation on every sensitive operation, not just at the entry point.
- Session tokens: use short-lived JWTs or opaque tokens with server-side revocation.
- Implement rate limiting on auth endpoints.

---

## Injection Vulnerabilities

| Vector | Prevention |
|---|---|
| SQL injection | Parameterised queries, ORM |
| NoSQL injection | Schema validation, typed queries |
| Command injection | Never construct shell commands from user input; use safe subprocess APIs |
| Path traversal | Normalise and validate file paths; reject `../` sequences |
| XSS | Escape output; use Content Security Policy headers |
| SSRF | Validate and restrict outbound URLs; block internal IP ranges |

---

## Secrets and Configuration

- Secrets are injected as environment variables at runtime.
- `.env` files are for local development only. They are in `.gitignore`.
- Use a secrets manager (AWS Secrets Manager, Vault, etc.) in staging and production.
- Rotate secrets on suspected compromise immediately.
- Audit secret access in production environments.

---

## Dependencies

- Pin dependency versions. Do not use floating ranges in production builds.
- Run a dependency vulnerability scanner (e.g. `npm audit`, `pip-audit`, `trivy`) in CI.
- Review new dependencies before adding them: check maintenance status, licence, and known CVEs.
- Keep dependencies up to date — unpatched vulnerabilities are the most common attack vector.

---

## Data Protection

- Identify which data is PII, sensitive, or regulated. Document it in `product.md`.
- PII must be encrypted at rest.
- Transmit all data over TLS. Never HTTP in production.
- Apply data minimisation: collect and retain only what is necessary.
- Implement retention policies. Data that should not exist cannot be breached.

---

## Security Review Triggers

Flag any change that touches these areas for explicit security review before merging:

- Authentication or authorisation logic
- Cryptography (do not implement custom crypto)
- Payment or financial data handling
- File upload or download handling
- External URL fetching or redirects
- New external dependencies
- Infrastructure or IAM configuration changes
- Admin or privileged endpoints

---

## When the AI Finds a Security Issue

Even when not asked to review for security, flag potential issues:

- State the vulnerability class (e.g. "This concatenates user input into a SQL query — SQL injection risk").
- Show the affected line(s).
- Propose the fix.
- Do not proceed with the original task until the issue is acknowledged.
