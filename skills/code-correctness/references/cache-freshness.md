---
description: Cache review. Check whether latency gains change visibility, freshness, auth, privacy, or correctness semantics.
---

# Cache & Freshness

Use this lens when a change affects caches, TTLs, memoization, replicas, async refresh, read models, batching, or any path that can return stale data.

Related lenses:

- [Reliability](./reliability.md)
- [Silent Failure](./silent-failure.md)
- [Schema Evolution](./schema-evolution.md)

## Review points

### 1. State the read contract

Every cached read path needs a contract for:

- read scope
- what may be stale
- maximum staleness by time or version
- invariants that still hold

This matters most for:

- auth
- privacy
- visibility
- ordering
- money
- inventory

Default posture:

- auth, privacy, entitlements, money, and inventory should not be stale
- post-save UX should at least give read-your-writes to the saving client or session
- static or public content can often use TTL-based caching

### 2. Cache keys are part of correctness

The key must include every input that can change the result.

Common misses:

- tenant, org, or user
- role or auth scope
- locale, timezone, or currency
- request shaping fields
- schema or API version

Common fixes:

- central key construction
- version token in the key
- normalized params
- shared base data plus per-user overlay instead of fully personalized cache entries

### 3. Invalidation is the hard part

Every write that changes what a read should return needs an invalidation or version-advance path.

Bad patterns:

- TTL used where prompt visibility matters
- only some write paths invalidate
- one cache layer invalidated while another still serves old data
- negative caching of "not found" with no safe create-visibility story

Common approaches:

- write-through
- cache-aside with explicit invalidation
- versioned or generation keys
- durable event-driven invalidation when fanout must be reliable

### 4. Serving stale data changes semantics

Stale-on-error and degraded modes are acceptable only under an explicit contract.

Requirements:

- origin failures are not hidden
- `max_stale` is bounded
- degraded responses do not break auth, privacy, revocation, money, or inventory rules

### 5. Stampedes are correctness and reliability issues

Watch for:

- many misses for the same key
- synchronized expiry
- refresh traffic that amplifies an outage

Mitigations:

- jittered TTLs
- single-flight or request coalescing
- stale-while-revalidate only when allowed by contract
- justified pre-warm of hot keys

### 6. Authorization and privacy override convenience

Never let caches leak data across principals.

Common failures:

- shared caches serving user-specific responses
- cached personalized renderings with no principal-aware keying
- slow revocation because cache entries outlive access changes

### 7. Multi-layer caches need traceability

If multiple layers can serve the response, be able to tell which layer won.

Useful debug fields:

- `X-Cache-Layer`
- `X-Cache`
- `X-Data-As-Of` or `X-Data-Version`
- `X-Cache-Key-Hash`

Do not expose raw keys or PII.

## Measurements to expect

At minimum:

- hit and miss rate by endpoint or read class
- served-stale rate, if stale serving exists
- invalidation success rate
- write-to-invalidation lag
- data age distribution
- replica lag or watermark when replicas or materialized views are involved

## Accept only if

- the freshness guarantee is explicit
- the staleness bound or reconciliation rule is explicit
- keying and invalidation are defined
- stale or degraded serving is visible
- cross-principal leakage is blocked
- there is a rollback path to authoritative reads