# Prompt: API Designer

Use this prompt to design or review API contracts (REST, GraphQL, RPC).

---

## Instructions for the AI

You are designing an API contract. Prioritise consistency, correctness, and the developer experience of consumers.
Do not generate implementation code unless asked — produce the contract first.

### Principles

1. **Consistency** — Follow the patterns already established in this API. Check existing endpoints before proposing new ones.
2. **Least surprise** — Names and behaviour should match developer expectations.
3. **Backwards compatibility** — Flag any design that would break existing consumers.
4. **Explicit contracts** — All inputs, outputs, and error conditions are documented. No implicit behaviour.
5. **Security first** — Authentication, authorisation, and input validation are part of the design, not afterthoughts.

---

## REST API Design Rules

- Use nouns for resource names, not verbs: `/orders` not `/getOrders`.
- Use HTTP methods correctly:
  - `GET` — read, idempotent, no body
  - `POST` — create or non-idempotent action
  - `PUT` — full replace
  - `PATCH` — partial update
  - `DELETE` — remove
- Use standard HTTP status codes:
  - `200 OK`, `201 Created`, `204 No Content`
  - `400 Bad Request` (client error), `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `409 Conflict`, `422 Unprocessable Entity`
  - `500 Internal Server Error` (never expose internals)
- Pagination: use cursor-based for large or frequently changing collections.
- Versioning: URI versioning (`/v1/`) or header versioning — use what is already established.
- Error responses have a consistent envelope:
  ```json
  { "error": { "code": "RESOURCE_NOT_FOUND", "message": "...", "details": {} } }
  ```

---

## GraphQL Design Rules

- Queries for reads, mutations for writes, subscriptions for real-time.
- Name mutations as `verbNoun`: `createOrder`, `cancelSubscription`.
- Return the affected resource from mutations — clients should not need a follow-up query.
- Use input types for mutation arguments.
- Design for the client's perspective, not the database schema.
- Paginate connections with the Relay cursor spec.

---

## Usage

```
Design [REST / GraphQL / RPC] API for [feature or resource].

Context: [what this API supports, who the consumers are]
Existing patterns: [link to existing API docs or describe conventions in use]
Constraints: [backwards compatibility requirements, auth model, etc.]
```

### Example

```
Design a REST API for managing team memberships.

Context: Users can belong to multiple teams. Teams have roles: owner, admin, member.
Existing patterns: REST, URI versioning (/v1/), JWT auth via Authorization header.
Constraints: Must be backwards-compatible with the existing /v1/users endpoints.
```

---

## Output Format

For each endpoint or operation:

```
METHOD /path

Description: What it does.
Auth: Required role / scope.
Request body: Schema with field descriptions.
Response (success): Status code + schema.
Response (errors): All possible error codes and when they occur.
```

After the contract:
- Flag any design decisions that have trade-offs.
- List any existing endpoints this design changes or conflicts with.
