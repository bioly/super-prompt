# Multi-Agent Patterns

Patterns for orchestrating multiple agents to handle complex, multi-step tasks.

---

## When to Use Multiple Agents

Use multiple agents when a task:
- Has clearly separable concerns that map to different contexts or permissions
- Benefits from parallel execution of independent sub-tasks
- Is long enough that a single context window is a constraint
- Has stages where one agent validates the output of another

Do not use multiple agents for tasks that are simpler done sequentially in one context.

---

## Core Patterns

### 1. Pipeline

Agents pass their output to the next agent in a fixed sequence.

```
Input → [Agent A] → [Agent B] → [Agent C] → Output
```

**When to use:** Multi-stage transformation where each stage has different context requirements.

**Example: Code generation pipeline**
```
Task description
  → Planner agent (produces implementation plan)
  → Coder agent (produces code from plan)
  → Reviewer agent (produces review from code)
  → Fixer agent (produces corrected code from review)
```

**Design rule:** Each agent in the pipeline should be able to run independently given its input. No hidden shared state between stages.

---

### 2. Parallelise-then-Merge

An orchestrator splits work into independent units, runs agents in parallel, then merges results.

```
              → [Agent A]  \
Input → [Orchestrator]       → [Merger] → Output
              → [Agent B]  /
```

**When to use:** Independent sub-tasks that can be run concurrently (e.g. reviewing multiple files, generating tests for multiple modules).

**Example: Multi-file review**
```
PR diff
  → Orchestrator splits diff by file
  → Reviewer agent runs on each file in parallel
  → Merger aggregates findings, deduplicates, and ranks by severity
```

**Design rule:** The units must be genuinely independent. If Agent A's output affects Agent B's work, use Pipeline instead.

---

### 3. Specialist Delegation

A general-purpose orchestrator delegates sub-tasks to specialist agents.

```
[Orchestrator]
    → /delegate security-check to [Security Agent]
    → /delegate test-gen to [Test Agent]
    → /delegate docs to [Docs Agent]
```

**When to use:** Complex features that require deep expertise in multiple areas simultaneously.

**Example: Feature implementation**
```
Orchestrator receives feature request
  → Delegates API contract design to API agent
  → Delegates implementation to Coder agent
  → Delegates test generation to Test agent
  → Delegates documentation to Docs agent
  → Reviews and assembles all outputs
```

**Design rule:** The orchestrator is a coordinator, not an implementer. It should not do the specialist's work.

---

### 4. Critic-Revise Loop

An agent produces output; a critic agent evaluates it; the original agent revises. Repeat until quality threshold is met.

```
[Producer] → output → [Critic] → feedback → [Producer] → ...
```

**When to use:** Tasks where quality is hard to define up front but easy to evaluate (generated tests, API designs, security reviews).

**Example: Test quality loop**
```
Test generator produces test suite
  → Critic evaluates: coverage of edge cases, test isolation, can tests fail?
  → If below threshold: Test generator revises based on feedback
  → Loop up to N iterations
  → Return final test suite + critic's assessment
```

**Design rule:** Set a hard iteration limit (typically 3). Infinite revision loops do not converge. If quality is not reached in N iterations, surface the problem to the human.

---

### 5. Verification Gate

A verifier agent must approve output before it proceeds to the next stage.

```
[Agent A] → output → [Verifier] → ✓ proceed / ✗ reject
```

**When to use:** High-stakes outputs (security changes, DB migrations, public API changes) where an automated check is valuable before human review.

**Example: Security gate**
```
Code change is generated
  → Security agent checks for vulnerabilities
  → If blockers found: halt and surface findings
  → If clear: proceed to PR creation
```

**Design rule:** The verifier must have clear, binary pass/fail criteria. Ambiguous verification produces no value.

---

## Orchestration Rules

- **Orchestrators are thin.** They route and coordinate; they do not implement.
- **Agents are stateless.** Pass all necessary context to each agent explicitly. Do not rely on shared memory.
- **Fail clearly.** If any agent in a pipeline fails, stop and surface the error with context. Do not silently skip stages.
- **Log the plan.** Before execution, the orchestrator should state which agents will run and in what order.
- **Hard limits on loops.** Any pattern that can loop must have an explicit iteration cap.
- **Human escalation path.** Every multi-agent flow must have a defined point at which it escalates to human review rather than continuing autonomously.

---

## Anti-Patterns

- **Chatty agents** — Agents that pass large unstructured blobs to each other rather than structured outputs. Define clean interfaces between agents.
- **God orchestrator** — An orchestrator that also implements logic. Extract to a specialist agent.
- **Silent failure** — An agent that fails to complete its task but passes partial output to the next stage. Fail loudly.
- **Unbounded loops** — A revision loop without an iteration limit or quality threshold definition.
