---
description: Test review. Check whether tests exercise real behavior, fail for real defects, and stay deterministic.
---

# Fake Coverage

Use this lens for tests, CI failures, flaky suites, mock-heavy changes, snapshots, and build changes that claim confidence without real coverage.

## Review points

### 1. Green must mean something real

A test should fail under a plausible defect in the behavior it claims to check.

Weak signals:

- "did not crash" as the main assertion
- tests that pass under obvious real bugs
- mocks that hard-code the answer
- snapshots updated without semantic review

### 2. Flakiness is a bug

Common causes:

- races
- uncontrolled time
- nondeterministic ordering
- real network or DNS
- shared mutable state
- leaked environment
- parallel tests without isolation

Bad "fixes":

- sleeps
- hidden retries
- timeout inflation
- quarantine with no path to removal

Prefer:

- explicit completion conditions
- controlled clocks
- isolated state per run
- hermetic dependencies by default

### 3. Mock boundaries, not truth

Use mocks for boundaries you do not own or cannot make cheap and deterministic.

Prefer:

- assertions on observable outcomes
- fakes that preserve the relevant contract
- boundary contract tests where drift in auth, schema, serialization, or protocol matters

Be skeptical when the test passes because the mock was told to return the expected result.

### 4. Snapshots are diffs, not proof

Snapshot tests are acceptable only when they are:

- small
- stable
- intentional

Also require at least one semantic assertion when correctness matters.

Useful supporting steps:

- normalize non-semantic fields
- sort collections before snapshotting
- explain why a snapshot change is correct

### 5. Default to hermetic tests

Typical order of preference:

1. in-process or in-memory dependency
2. local ephemeral dependency
3. controlled fake server
4. isolated environment per run
5. record/replay with strict versioning
6. real external system only for labeled smoke, E2E, provider-contract, certification, or sandbox checks

If a test is non-hermetic, label it and explain why.

### 6. Assertion strength matters

Every test should assert at least one correctness-relevant invariant.

Good invariant classes:

- domain invariants
- structural invariants
- behavioral invariants

### 7. Control nondeterminism

Control the sources that affect the behavior under test:

- time
- randomness
- ordering
- environment

Waiting and timeouts may bound a test. They do not prove correctness.

### 8. Failure output should shorten debugging

A failing test should make the broken invariant obvious.

Useful failure detail:

- the invariant
- expected vs actual
- for waiting tests: last observed state, elapsed time, attempt count

## Temporary exceptions

A weak or non-hermetic test is acceptable only if:

- the test type is explicit
- stronger isolation is not practical or not enough
- waiting is bounded
- failures are actionable
- flake rate is visible
- there is an exit condition for replacement or tightening

## Accept only if

- the test would fail under a plausible real bug
- the test checks production-relevant behavior, not just lack of an exception
- sleeps, retries, and timeouts are bounds, not proof
- mocks sit at a boundary rather than replace the truth under test
- snapshots are paired with semantic checks
- relevant nondeterminism is controlled
- failure messages explain what broke