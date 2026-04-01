---
description: Side-effect review for duplicate-safe retries, deduplication, unknown outcomes, and at-least-once delivery.
---

# Idempotency

Use this lens for retryable writes, queue consumers, webhooks, billing, notifications, multi-step workflows, and any operation where a timeout can hide the outcome.

Related lenses:

- [Reliability](./reliability.md)
- [Silent Failure](./silent-failure.md)

## Review points

### 1. Exactly once is not the transport default

Do not rely on:

- exactly-once delivery
- clients never retrying
- FIFO preventing duplicates
- timeout implying failure
- same message body meaning same intent
- "retry with a new ID" after an unknown outcome

Treat exactly-once as an application-level effect guarantee.

### 2. Classify the operation first

- **natural idempotency**: repeating the operation leaves the intended state unchanged and creates no duplicate side effect
- **synthetic idempotency**: the operation has side effects and needs explicit deduplication

Classify by the real effect, not by the shape of the endpoint.

### 3. Deduplication keys identify logical intent

Every non-naturally-idempotent operation needs a stable key.

The key must be:

- unique per logical operation
- stable across retries
- different for different intents
- scoped correctly
- bound to the original request parameters or business intent

For consumers, prefer message IDs or business operation keys over body equality.

### 4. Record the first outcome durably enough

Minimum stored state:

- deduplication key
- request fingerprint or equivalent intent binding
- execution state: `in_progress`, `succeeded`, `failed`, or `unknown`
- response or a resolvable outcome reference
- timestamps
- lease or owner metadata for in-progress work when needed

Durability should match the cost of duplication.

### 5. Keep failure classes distinct

At minimum, separate:

- pre-execution rejection
- definitive business outcome
- transient execution failure
- unknown outcome

Do not trap a key forever in a failed state unless that is the explicit contract.

### 6. In-progress means one live execution

A duplicate request must not start a second execution while the first one is still active.

Valid duplicate behavior during active execution:

- return `processing`
- ask the caller to retry later
- wait for completion and return the stored result

`in_progress` needs recovery: lease expiry, heartbeat, owner token, or reconciliation.

### 7. Deduplication windows need a contract

Define:

- how long the key remains valid
- whether the window covers realistic retries, redelivery, and manual replay
- what happens after expiry

Common policies after expiry:

- treat reuse as a new operation
- reject reuse
- require a new key

### 8. Timeout means unknown outcome

If the caller cannot prove non-execution, the result is `unknown`, not `failed`.

Requirements:

- retries reuse the same key
- callers can query final state when sync completion is not guaranteed
- caller state keeps `unknown` distinct from `failed`

Acceptable resolution paths:

- status endpoint
- operation resource
- workflow state store
- callback or webhook
- reconciliation query

### 9. Workflows need per-step idempotency

For multi-step work:

- checkpoint steps durably
- derive per-step keys from workflow identity and step identity
- make completion checks cheap
- define compensation for non-idempotent external effects

### 10. At-least-once consumers must deduplicate

Consumers must be naturally idempotent or record handled work safely under concurrency.

Prefer:

- business state and processed-message record in one transaction where possible
- outbox for reliable downstream emission after state change

## Minimal procedure

1. Determine the logical operation identity.
2. Bind a stable deduplication key to that identity and the original parameters.
3. Reserve or look up the key before executing the side effect.
4. If the key already exists, return the stored final outcome, a valid in-progress response, or reject if the parameters do not match.
5. Execute the side effect only if this attempt owns the reservation.
6. Persist the final outcome or explicit `unknown` state.
7. Recover abandoned in-progress work with the defined lease or reconciliation rule.

## Temporary exceptions

An idempotency workaround is acceptable only if it states:

- what can duplicate
- blast radius
- detection
- reconciliation path
- owner
- exit condition

## Accept only if

- the same logical request with the same key produces one effect
- a duplicate during active execution does not start another execution
- reusing a key with different parameters is rejected
- window-expiry behavior is explicit
- unknown outcomes are visible and resolvable
- workflows checkpoint or deduplicate each side-effecting step