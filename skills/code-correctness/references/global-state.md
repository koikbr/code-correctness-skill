---
description: Mutable-state review for hidden context, ownership, initialization, explicit dependencies, and test isolation.
---

# Global State

Use this lens when correctness depends on shared mutable state, globals, singletons, import-time setup, hidden context, or runtime config read from anywhere.

Related lenses:

- [Flag Debt](./flag-debt.md)
- [Single Source of Truth](./single-source-of-truth.md)

## Terms

- **ambient state**: state code can observe without that dependency appearing in the local API
- **owner**: the component that initializes, mutates, validates, and tears down a mutable state
- **snapshot**: an immutable value published to readers

## Review points

### 1. Hidden mutable context is a correctness risk

Flag:

- unrelated code changing behavior through shared state
- import order affecting behavior
- tests that pass alone but fail in suite
- functions depending on invisible current user, tenant, locale, or timezone

Hidden mutable ambient state should not be authoritative for business correctness.

### 2. Every mutable state needs one owner

One owner should define:

- initialization
- invariants
- mutation rules
- lifecycle
- teardown

Everyone else should get snapshots, read-only views, or narrow capability methods.

### 3. Make dependencies explicit

Prefer:

- constructor injection
- parameter injection
- small typed context objects
- factory or builder wiring at the composition root

Avoid service-locator lookups in business logic.

### 4. Default immutable, mutate locally, publish snapshots

Flag:

- functions mutating input parameters
- shared maps or lists handed to multiple consumers
- memoization implemented by mutating domain objects

Prefer:

- builder then freeze
- copy-on-write
- returning new values
- explicit mutation APIs when mutation is required

If readers depend on replaced snapshots, publish them through a visibility mechanism appropriate to the runtime.

### 5. Initialization must be explicit

Flag:

- imports triggering I/O, env parsing, or background work
- lazy global init guarded by ad hoc booleans
- invalid config discovered after dependent work has already started

Require:

- explicit initialization
- idempotent init where repeated entry is possible
- init order owned by the composition root
- fail-fast behavior for invalid config

### 6. Tests should build fresh worlds

Flag:

- teardown that says "reset singleton"
- test suites that require serial order
- tests whose outcome depends on prior test state

Require:

- isolated mutable state
- fresh component graphs for testable paths
- no cross-test mutation through globals on business-critical paths

### 7. Shared config globals are a footgun

Prefer immutable config snapshots.

If config changes at runtime:

- access it through an explicit dependency
- scope it
- validate it
- make it observable

Do not scatter raw global reads throughout the codebase.

## Temporary exceptions

Shared or hidden state is acceptable only if it states:

- what is shared
- why it is needed
- who may read and write it
- concurrency model
- cleanup path, especially for tests
- exit plan

## Accept only if

- business behavior does not depend on import order
- required dependencies are visible in constructors, parameters, or typed context
- each mutable state has a clear owner
- readers cannot mutate state they do not own
- initialization happens explicitly before dependent work
- tests remain stable in isolation and in parallel
- dynamic config access is explicit, scoped, and observable