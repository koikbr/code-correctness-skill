---
description: Timing review for code that depends on delay, ordering luck, environment defaults, or rollout coincidence.
---

# Timing Correctness

Use this lens when code waits, polls, orders work, depends on default environment, or assumes a particular rollout sequence.

Related lenses:

- [Concurrency](./concurrency.md)
- [Time & Locale](./time-locale.md)
- [Schema Evolution](./schema-evolution.md)

## Review points

### 1. Delay is not proof

Flag code whose correctness depends on:

- sleeping for completion
- waiting a fixed time for visibility
- wall-clock delay to establish ordering
- scheduler luck
- host load behaving nicely

Replace delay-based reasoning with an observable predicate, explicit contract, or enforced invariant.

### 2. Use time as a bound, not as logic

Use time for:

- deadlines
- timeouts
- retry budgets
- circuit breakers
- backoff pacing

Do not use time to prove:

- completion
- visibility
- ordering
- correct sequencing

Use a monotonic clock for local elapsed-time measurement. Use wall clock for user-facing and calendar rules.

Across process boundaries, propagate a remaining budget or explicit deadline contract, not raw monotonic values.

### 3. Prefer signals and state

If the code must wait for `X`, define `X` as an observable event or predicate.

Good mechanisms:

- completion events
- futures, promises, or tasks
- explicit state machines
- readiness predicates checked until success or deadline

### 4. Polling must be engineered

Polling is acceptable only when no better signal exists or the contract is explicitly predicate-based.

A compliant polling loop needs:

- a precise predicate
- a deadline
- backoff plus jitter
- monotonic elapsed-time measurement
- diagnostics
- instrumentation for attempts, latency, and timeout rate

### 5. Ordering is a contract

Do not assume:

- database rows come back in insertion order
- unordered collection iteration is stable
- transports preserve order unless documented
- timestamps alone define a total order

When order matters:

- sort explicitly
- use sequence numbers or versions
- tolerate duplicates and reordering where the transport can do that
- define a tie-breaker when timestamps can collide

### 6. Correctness-affecting constants need names

Timeouts, polling intervals, retry limits, queue limits, and concurrency limits are policy.

They should be:

- named
- justified
- bounded
- validated
- overridable where appropriate

### 7. Environment assumptions are part of the contract

Flag behavior that depends on:

- machine timezone or locale
- nondeterministic iteration or serialization
- unseeded randomness
- host-specific defaults

Prefer explicit injection, seeding, sorting, and canonicalization.

### 8. Assume version skew

During deploys and migrations, old and new behavior can coexist.

Do not rely on lockstep rollout.

Use additive-first evolution and safe overlap.

## Minimal procedures

### For timing or ordering workarounds

Require:

1. explicit assumption
2. bounded wait
3. operational visibility
4. blast-radius control
5. exit plan

### For timeout-based tests or joins

1. wait using the real synchronization mechanism
2. on timeout, emit state, stacks, lock graph, or equivalent evidence
3. fail or recover according to the liveness policy

## Accept only if

- every wait names an observable predicate or event
- sleeps and delays are not required for correctness
- every timeout has a budget source and bounded expiry behavior
- polling includes predicate, deadline, backoff, jitter, and instrumentation
- any order-dependent behavior has an explicit ordering contract or explicit sort
- environment-sensitive behavior makes those inputs explicit
- rollout-sensitive behavior is safe under mixed versions