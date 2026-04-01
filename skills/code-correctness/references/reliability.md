---
description: Runtime reliability review for bounded resources, deadlines, backpressure, retries, cleanup, overload, and recovery.
---

# Reliability

Use this lens for pools, queues, background work, high fan-out flows, saturation incidents, dependency failures, deadlines, and recovery paths.

Related lenses:

- [Idempotency](./idempotency.md)
- [Silent Failure](./silent-failure.md)

## Review points

### 1. Bound work before expensive execution

Requirements:

- concurrency is capped before costly work starts
- queues, pools, and in-flight work have hard limits
- saturation behavior is explicit

Valid saturation policies:

- reject
- shed
- degrade
- bounded spill to another path that is itself observable and bounded

An unbounded in-memory queue is not backpressure. It is delayed failure.

### 2. Every resource needs ownership and cleanup

Watch for:

- cleanup only on the happy path
- reliance on GC while native resources are open
- pools with no max size or leak detection
- background work outliving the parent request with no explicit ownership transfer

Prefer:

- RAII / `defer` / `try-finally` / context managers
- central resource creation with limits and instrumentation
- startup-time validation of unsafe bounds

### 3. Every external wait needs a deadline

Rules:

- every external call has a deadline
- queue waits have deadlines
- per-hop budgets are smaller than the end-to-end budget
- retries consume the same budget
- cancellation stops further work and triggers cleanup

For streams, use idle timeouts, heartbeats, max lifetime, and explicit flow control.

### 4. Isolate sick dependencies

Use:

- circuit breakers per dependency or operation class
- meaningful trip signals such as timeouts, connect failures, and dependency `5xx`
- minimum volume before tripping
- sane half-open probes
- bulkheads with per-dependency concurrency limits

When a breaker is open, fail fast. Do not queue doomed work.

### 5. Retry control matters

Rules:

- backoff with jitter
- bounded retry count or budget
- one retry owner layer
- do not retry non-retryable classes

Never retry side effects unless the operation is duplicate-safe.

### 6. Connection and handle hygiene is part of correctness

Watch for:

- one client or pool per request
- pool saturation with no acquisition timeout
- response bodies or streams left unread and unclosed

Prefer:

- client reuse
- capped pools
- acquisition timeouts
- explicit close or discard rules
- pool instrumentation

### 7. Observe resource pressure

Minimum metrics:

- in-flight requests
- queue depth
- age of oldest queued work or enqueue-to-start delay
- pool utilization
- acquisition wait time and timeouts
- deadline or timeout counts
- breaker state transitions
- retry counts
- memory
- file descriptors
- open connections
- thread or task counts

Prefer state-transition logs over per-request noise.

## Temporary exceptions

A reliability workaround is acceptable only if it states:

- what can saturate, drop, delay, or leak
- blast radius
- bounds
- detection
- mitigation or rollback
- owner
- exit condition

## Accept only if

- queues, pools, and in-flight work are bounded
- saturation behavior is explicit
- every resource has an owner and unskippable cleanup path
- external calls and queue waits have deadlines
- cancellation releases resources promptly
- breakers and bulkheads isolate bad dependencies
- retries are bounded, jittered, and owned by one layer
- pressure and leak signals are observable
- tests cover overload, dependency failure, shutdown, and recovery