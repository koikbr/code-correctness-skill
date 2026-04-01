---
description: Contract review for illegal states, lifecycle rules, preconditions, postconditions, and externally visible invariants.
---

# Contract Invariants

Use this lens when code introduces or changes state machines, constructors, validators, update paths, multi-field rules, or cross-service contracts.

Related lenses:

- [Data Representation](./data-representation.md)
- [Schema Evolution](./schema-evolution.md)
- [Silent Failure](./silent-failure.md)

## Review points

### 1. Illegal states should be hard or impossible to represent

Prefer types that exclude invalid combinations.

Watch for:

- boolean combinations with forbidden states
- partially initialized objects
- nullable fields that "must be set later"
- sentinel values standing in for real states

Fixes:

- enums or tagged unions
- constructors or factories that reject incomplete data
- explicit state types
- non-null required fields where the type system allows it

Tests may bypass constructors only for boundary-hardening, corruption handling, deserialization checks, or recovery tests. The reason should be explicit.

### 2. Lifecycle rules must live in one place

If an entity has states, valid transitions should be explicit and centrally enforced.

Flag:

- free-form `status` assignment
- state checks spread across callers
- different services assuming different valid transitions
- string comparison used as the state machine

### 3. Preconditions belong at boundaries

Reject bad input at the boundary where it enters.

Flag:

- deep defensive cleanup that silently repairs invalid input
- functions that "work" with bad input by accident
- failures surfacing far from the source of bad input

Good failures say what was wrong, what was expected, and which field or assumption failed.

### 4. Postconditions must be defined

After success, the caller should know what is guaranteed.

Flag:

- operations that report success while the claimed invariant may still be false
- partial updates with no defined recovery model
- callers forced to inspect unrelated state to tell whether the operation worked

Where atomicity is impossible, make partial success, deferred completion, retry, idempotency, compensation, and reconciliation behavior explicit.

### 5. Invariants must hold whenever outside code can observe state

Flag:

- public setters that can break consistency
- `init()` patterns that expose half-built objects
- invariant rules that exist only in comments
- tests normalizing invalid construction paths that production cannot take

### 6. Assumption drift is predictable

When callers and callees evolve separately, implicit assumptions drift.

Flag:

- "the caller never sends X" with no enforcement
- provider and consumer disagreeing on required fields
- mocks or fixtures encoding rules no longer true in production

Prefer shared schemas, contract tests, compatibility tests, and validation at trust boundaries.

### 7. Defensive vs offensive handling depends on the boundary

At external trust boundaries:

- validate
- sanitize where appropriate
- reject malformed or invalid data with clear errors

Inside trusted domains:

- assert invariants
- treat impossible states as bugs
- make invariant breaches visible

Bad patterns:

- trusted code silently absorbing invalid state
- external boundaries crashing on routine input variation that should be handled as input error

## Temporary exceptions

A temporary weak invariant is acceptable only if it states:

- what remains weak
- who can rely on it
- compensating checks
- how usage is measured
- migration plan for callers
- removal condition

## Accept only if

- illegal states are unrepresentable where practical, otherwise rejected at construction or boundaries
- lifecycle transitions are explicit and central
- preconditions are enforced at boundaries
- postconditions are defined and testable
- externally visible state preserves invariants
- cross-team contracts are explicit and checked