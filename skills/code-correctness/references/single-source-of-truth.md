---
description: Drift review for duplicated business rules, validation, calculations, policy, and protocol logic.
---

# Single Source of Truth

Use this lens when the same normative rule appears in more than one place: service, client, worker, migration, script, fixture, or test.

Related lenses:

- [Global State](./global-state.md)
- [Schema Evolution](./schema-evolution.md)

## Terms

- **same rule**: the same decision procedure or semantic contract, even if expressed in different layers or languages
- **authority**: the place where that rule is defined
- **consumers**: every code path or workflow that depends on the rule
- **forked truth**: correctness depends on multiple copies staying aligned

## Review points

### 1. Classify the rule before choosing the authority

Common cases:

| Rule type | Typical clones | Preferred authority |
| --- | --- | --- |
| Business policy | services, jobs, UI | domain module or policy service |
| Validation | frontend, backend, workers | shared schema or validators, with server authority |
| Mapping and serialization | producers, consumers | schema or IDL plus generation and fixtures |
| Security policy | UI, gateway, service | server-side evaluation |
| Derived computation | DB, app, batch | one authoritative implementation or persisted derivation with provenance |

### 2. Use the smallest authority that prevents drift

A single authority can be:

- a shared module
- a service
- a schema or IDL
- data-layer constraints
- generated or computed columns

Do not centralize more than needed.

### 3. Some duplication is acceptable; silent divergence is not

Usually acceptable:

- client-side validation used only for quick feedback
- generated duplication from one source
- temporary compatibility shims with drift checks

Not acceptable:

- client-side authorization treated as authoritative
- duplicated authorization logic that can drift
- financial or policy computation duplicated without compatibility checks
- protocol handling with divergent edge-case behavior

### 4. Version skew can create effective drift

If old and new consumers coexist:

- define the skew window
- use additive-first contract changes
- verify compatibility with canonical fixtures or golden vectors

### 5. Tests must not become a rival authority

Flag:

- tests reimplementing the algorithm under test
- expected values produced by the same logic as production
- fixtures encoding business rules with no known owner or source

Prefer:

- curated canonical examples
- invariants
- differential tests during replacement work

### 6. Config semantics can fork too

If multiple components interpret the same config differently, the config has forked truth.

Shared config needs either:

- one evaluator
- or one executable specification with explicit schema and precedence rules

## Temporary duplication

Temporary duplication is acceptable only if:

- the duplicate rule is named
- one owner is responsible
- drift is measurable
- scope is bounded
- the sunset condition is clear
- new clones are blocked during the cleanup window

## Accept only if

- the authority is identifiable
- consumers are identifiable
- two valid paths cannot silently return different answers for the same input
- supported version skew is intentional and safe
- tests validate the authority rather than clone it
- temporary duplication has drift detection and a removal plan