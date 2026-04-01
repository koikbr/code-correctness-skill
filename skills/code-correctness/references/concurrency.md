---
description: Concurrency review for ownership, synchronization, visibility, locks, atomics, async boundaries, and liveness.
---

# Concurrency

Use this lens when code shares mutable state across threads or tasks, uses locks or atomics, mixes blocking and async work, or has deadlock, race, or starvation risk.

Related lenses:

- [Timing Correctness](./timing-correctness.md)
- [Global State](./global-state.md)
- [Reliability](./reliability.md)

## Review points

### 1. Pick an ownership model before picking a primitive

Prefer one of:

- single-writer or actor ownership
- thread or task confinement
- immutability with snapshot replacement
- shared mutable state behind one documented synchronization boundary

Do not start with "add a lock" before deciding who owns the state.

### 2. Every cross-thread read and write needs a visibility story

A delay does not create a happens-before edge.

Use primitives that do:

- lock and unlock
- release and acquire atomics
- condition variables
- semaphores
- channels or queues
- joins or barriers

If readers depend on immutable snapshots, publish them through a mechanism that establishes visibility on the target runtime.

### 3. Prefer blocking coordination over busy-wait

Default to condition variables, semaphores, channels, futures, or awaitables.

Spinning is acceptable only if:

- the expected wait is very short
- the spin is bounded
- there is a fallback to blocking
- spin behavior is instrumented

### 4. Keep locks small and legible

Require:

- clear statement of what each lock protects
- minimal critical sections
- no blocking I/O, sleep, or `await` while holding an unsafe lock
- no arbitrary callback or user code while locked

A giant lock is acceptable only when contention is measured acceptable and redesign criteria are defined.

### 5. Write down lock ordering

If multiple locks can be held together:

- define a global order
- enforce it in review, assertions, or tooling
- reject nested locking that violates the order

### 6. Atomics need a documented protocol

Use locks or message passing when invariants span multiple fields.

If atomics are used, document:

- publish and consume protocol
- required memory ordering
- lifetime and reclamation strategy for lock-free structures
- ABA handling where CAS-based algorithms need it

Do not use atomic flags in polling loops as hand-rolled condition variables.

### 7. Keep async and blocking separate unless bridged on purpose

Require:

- async-aware primitives in async code
- blocking or CPU-heavy work isolated from the event loop
- no unsafe `await` while holding a lock
- structured-concurrency mechanisms so lifetimes and cancellation are explicit

### 8. Liveness is part of correctness

Review for:

- deadlock
- livelock
- starvation
- hot locks
- shutdown hangs

Use:

- lock ordering
- bounded waits for detection and recovery
- fair queues or locks where starvation is plausible
- partitioning or sharding where one hotspot dominates

Timeouts detect or bound a hang. They do not fix the underlying liveness bug.

## Concurrency workaround gate

A workaround is acceptable only if it states:

1. who may call concurrently
2. which invariant it protects
3. how progress is guaranteed or bounded
4. which diagnostics appear on failure
5. how contention, retries, or timeout rate are measured
6. removal condition

## Minimal procedures

### Snapshot before notify or callback

When notification depends on shared state:

1. capture the required snapshot while holding the lock
2. release the lock
3. notify or call back using the snapshot

### Bound waits with diagnostics

When a timeout exists in a liveness path:

1. wait using the normal sync mechanism
2. on timeout, emit stacks, lock graph, state, or equivalent evidence
3. fail or recover according to the policy

## Accept only if

- ownership of each mutable state is explicit
- each cross-thread or cross-task read or write has a defined synchronization edge
- locks state what they protect
- multi-lock paths follow a documented order
- spinning is bounded and justified
- atomics use a documented protocol
- async code does not block the event loop or await inside unsafe critical sections
- timeout-based tests and joins emit enough evidence to distinguish deadlock, starvation, and slow progress