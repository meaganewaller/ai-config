---
slug: performance
version: 1
tags: [rails, backend, systems, optimization]
---

# Performance & Scalability Guidelines

## 1. Measure First

- Never optimize based on intuition alone.
- Ask: What is slow? How slow? Compared to what?
- Prefer concrete metrics (time, memory, allocations, query count).
- Identify the real bottleneck before proposing changes.

If no measurements are available, recommend how to measure.

---

## 2. Optimize the Right Layer

Performance problems usually live in one of:

- Database (N+1 queries, missing indexes, bad query plans)
- Network (chatty APIs, large payloads)
- Serialization (heavy JSON building)
- Allocation-heavy Ruby code
- Lock contention / blocking IO

Identify which layer is responsible before suggesting changes.

---

## 3. Database Discipline (Rails Context)

- Always check for N+1 queries.
- Prefer eager loading (`includes`, `preload`) when appropriate.
- Ensure indexed columns are used in WHERE / JOIN clauses.
- Avoid SELECT * when large tables are involved.
- Think about query shape, not just query count.

When discussing a query, reason about:
- cardinality
- selectivity
- expected row count

---

## 4. Avoid Premature Optimization

Do not:

- Micro-optimize clean, readable code without evidence.
- Replace expressive code with obscure cleverness for marginal gains.
- Suggest caching before validating necessity.

Prefer:

- Clarity first
- Then targeted optimization

---

## 5. Memory & Allocation Awareness

In Ruby:

- Avoid building large intermediate arrays unnecessarily.
- Consider streaming when dealing with large datasets.
- Avoid repeated expensive object creation in tight loops.

In general:
- Be aware of algorithmic complexity (O(n), O(n²), etc.)
- Avoid quadratic behavior over large collections.

---

## 6. Concurrency & Throughput

- Distinguish latency vs throughput.
- Identify blocking operations.
- Be explicit when recommending background jobs.

If suggesting async work, clarify:
- Idempotency requirements
- Failure modes
- Retry implications

---

## 7. Caching Strategy

If recommending caching, specify:

- What layer (DB, app, HTTP, fragment)
- Invalidation strategy
- TTL assumptions
- Consistency tradeoffs

Never suggest cache without invalidation plan.

---

## 8. Tradeoffs Over Absolutes

Performance improvements always involve tradeoffs:

- Complexity
- Maintainability
- Memory
- Consistency
- Cost

Explicitly articulate tradeoffs.

---

## Response Style for Performance Reviews

When analyzing code:

1. Identify likely bottleneck
2. Suggest how to measure
3. Propose improvement
4. Explain tradeoffs
5. Recommend validation

Be pragmatic, not dogmatic.
Avoid vague statements like "this might be slow" without reasoning.