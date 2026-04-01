---
description: Failure-path review for swallowed errors, hidden fallback, false success, and async work with no durable outcome.
---

# Silent Failure

Use this lens when operations can fail, retry, fall back, enqueue work, or return partial results.

Related lenses:

- [Reliability](./reliability.md)
- [Observability Gaps](./observability-gaps.md)
- [Idempotency](./idempotency.md)

## Outcome model

Cross-boundary operations should expose the relevant states directly:

- `Success`
- `Accepted(tracking)`
- `Partial(details)`
- `Failed(code)`
- `Unknown(tracking)`

Do not force callers to infer these from logs.

## Review points

### 1. Failure semantics are part of the contract

Flag:

- `ok=true` after dropping work
- transport success with business failure hidden in an ad hoc payload unless that is the stated contract
- async "it will retry later" with no durable tracking handle

### 2. Do not swallow exceptions

Caught errors must be:

- propagated
- converted to an explicit outcome
- retried within a visible budget
- compensated with explicit signaling

Flag:

- `catch { log; continue }` on correctness-sensitive paths
- detached async tasks with no error sink
- repeated terminal logging at every layer

Inner layers should add context. One layer should own outward reporting.

### 3. Silent fallback changes meaning

Fallbacks are acceptable only when declared invariants still hold and the degraded path is visible.

Examples to flag:

- `[]` meaning both "no results" and "failed to compute"
- stale data returned as if current
- default values hiding upstream failure

### 4. Best-effort writes need a receipt

A write path should produce one of:

- durable success
- explicit failure
- accepted plus durable tracking handle
- structured partial success
- explicit unknown outcome

Flag:

- batch success with hidden item failures
- enqueue reported as success even when full queues can drop
- timeout treated as definite failure when the real outcome is unknown

### 5. Retries must not erase the original failure

Require:

- retry only retryable classes
- explicit retry budgets
- visible terminal artifact after exhaustion
- metrics for retry attempts and exhausted retries

Timeout or lost acknowledgement means `Unknown` unless non-execution is proven.

### 6. Do not turn errors into empty values

`[]`, `0`, `false`, `null`, and similar sentinels should not double as failure markers unless the contract says so and callers can still distinguish the states they need.

### 7. Async work needs a durable error sink

Every async execution context must define:

- where failures go
- how operators or callers inspect terminal state
- retry bound
- escalation behavior

## Temporary exceptions

A degraded path may ship only if it states:

- the blind spot or semantic downgrade
- the risk
- bounds
- mitigation
- missing contract or instrumentation
- owner
- removal condition

## Accept only if

- callers can distinguish success, accepted, partial, failed, and unknown where needed
- exceptions are not swallowed
- fallback does not silently impersonate success
- retries are bounded and visible after exhaustion
- async work has a durable failure sink and inspectable terminal state