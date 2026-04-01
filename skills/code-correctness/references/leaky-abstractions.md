---
description: Boundary review for APIs that leak internal sequencing, protocol details, storage quirks, or hidden rituals.
---

# Leaky Abstractions

Use this lens when reviewing wrappers, SDKs, service interfaces, orchestration APIs, long-running jobs, or modules that callers must "know how to use".

Related lenses:

- [Contract Invariants](./contract-invariants.md)
- [Single Source of Truth](./single-source-of-truth.md)

## Review points

### 1. Safe usage should be the easy path

Flag APIs that require callers to remember a special order or hidden setup step.

Prefer:

- one safe operation instead of a fragile sequence
- builders
- typed state transitions
- scoped resource management
- explicit handles

If callers need to read internals to use the API safely, the boundary is weak.

### 2. Thin wrappers need an honest contract

A pass-through wrapper is fine if it is explicit about being a pass-through.

It is not a useful abstraction if it only renames vendor calls while still forcing callers to understand vendor error modes, retry rules, or edge cases.

Stronger boundaries are needed when callers need:

- domain outcomes instead of transport codes
- stability across vendor changes
- insulation from vendor quirks

### 3. Public constraints are fine; hidden rituals are not

Some constraints belong in the public contract:

- consistency model
- latency or cost class
- rate limits
- retry safety
- idempotency requirements
- ordering guarantees
- pagination rules

These are acceptable when explicit and stable.

A leak exists when callers must discover them by accident or infer them from implementation details.

### 4. Do not leak required protocol knowledge

Flag:

- sentinel values that trigger internal modes
- callers forced to close, flush, finalize, or commit with no scoped enforcement
- "if you see error X, do Y" when the owning layer could do Y
- undocumented polling rituals for long-running work

Prefer:

- a single operation
- explicit state models
- language-appropriate scoped cleanup
- job handles, status resources, streams, callbacks, or webhooks for long-running work

### 5. Keep observability without breaking the boundary

Callers and tests reach into internals when the public surface is too opaque.

Better options:

- status or probe APIs
- structured logs in domain terms
- metrics for externally meaningful states
- correlation IDs that cross boundaries

Observability should describe contract-visible reality, not force inspection of wiring.

## Temporary exceptions

A temporary boundary leak is acceptable only if it states:

- what is exposed
- who may depend on it
- guardrails
- how usage is measured
- compatibility plan during migration
- removal condition

## Accept only if

- callers can use the boundary safely without reading internals
- sequencing rules are enforced by API shape or stated plainly
- callers are not forced to reproduce internal retry, pagination, storage, or partitioning behavior unless that behavior is intentionally public
- results are returned at the right semantic level for the audience
- observability exists without breaking the abstraction