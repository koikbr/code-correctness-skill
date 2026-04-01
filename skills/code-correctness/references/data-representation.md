---
description: Data model review for ambiguous fields, sentinels, overloaded values, numeric semantics, and canonical representation.
---

# Data Representation

Use this lens for schemas, domain models, APIs, events, migrations, ETL, and integrations where correctness depends on what a value means.

Related lenses:

- [Schema Evolution](./schema-evolution.md)
- [Cache & Freshness](./cache-freshness.md)

## Terms

- **authoritative source**: the system of record for a concept
- **canonical representation**: the internal form business logic should use
- **derived representation**: a recomputable copy for indexing, performance, or presentation
- **trust boundary**: a boundary where data must be parsed and validated before use
- **missing**: absent
- **unknown**: syntactically valid but not yet recognized
- **invalid**: malformed or unsafe
- **conflicting**: two representations disagree

## Review points

### 1. Meaning must be explicit

Flag:

- the same value meaning different things by tenant, version, source, or flag
- special values carrying state such as `0`, `-1`, empty string, or `NULL`
- business logic based on string parsing
- product logic keyed off specific IDs

When legacy ambiguity already exists:

- keep the legacy form only for compatibility
- add a typed canonical projection
- confine legacy parsing to adapters
- require business logic to use the canonical projection

### 2. Types are contracts

Where the distinction matters, use distinct types or distinct fields.

High-risk areas:

- money
- identity and permissions
- time and scheduling
- lifecycle and state

Guidelines:

- replace generic `string` and `int` with domain types where practical
- use named enum values, not raw codes
- use tagged unions when only one shape is valid at a time
- use separate fields when multiple facts may be true at once
- use nullable fields only when null has exactly one meaning
- parse once at a trust boundary, then operate on validated values

### 3. Sentinel values hide state

Common sentinels:

- `0` for unlimited or none
- `-1` for unknown
- empty string for missing
- `NULL` for several different states
- `9999-12-31` for never

Represent `missing`, `unknown`, `invalid`, and `not_applicable` distinctly when product logic depends on the difference.

For partial updates, distinguish:

- field omitted
- field set to `null`
- field set to a value

Do not collapse those into one meaning unless the contract says they are the same.

### 4. One field, one concept

Flag:

- `status` driving unrelated subsystems
- `type` used as ad hoc schema switching
- JSON blobs carrying core domain semantics with no schema
- foreign keys that point to different entity kinds based on hidden context

Prefer:

- `lifecycle_status`, `billing_status`, `shipping_status`
- explicit discriminators for real polymorphism
- bounded and validated JSON only for flexible metadata

### 5. Codes need names, ownership, and safe unknown handling

Flag:

- `if status == 7`
- duplicated code tables drifting across services
- partner mappings scattered through business logic

Require:

- one owning contract
- shared code generation or shared validation where possible
- centralized partner and legacy mappings
- safe handling of unknown external values

Unknown values should be preserved where possible, not collapsed into a meaningful default.

### 6. IDs are identifiers, not business policy

Flag:

- admin user is ID 1
- special tenant is 999
- behavior keyed off a specific UUID embedded in product logic

Special behavior should come from explicit roles, policies, or configuration.

### 7. One authority per concept

For each important concept, define:

- authoritative source
- canonical representation
- derived representations
- rebuild path for derived state

If derived values are stored, record provenance such as rule version, input hash, or derivation time.

### 8. Representation changes need explicit transition rules

When old and new forms overlap, define:

- which form is authoritative
- which form business logic uses
- precedence when both are present
- how mismatches are detected and surfaced
- when the old form is removed

Use add-first, read-both, remove-last for overlapping transitions.

Do not prefer the new representation only because it is present. Prefer it only if it satisfies the contract and transition rule.

### 9. Validation is part of correctness

Require validation at every trust boundary.

Good controls:

- API and consumer validation
- ETL and import validation
- storage constraints such as `NOT NULL`, `CHECK`, `FOREIGN KEY`, `UNIQUE`
- domain invariant checks in application code

Flag:

- best-effort parsing that silently coerces
- mapping unknown external values to a normal business value
- partially valid records written with "we will fix it later"
- raw strings re-parsed deep in business logic

## Canonical rules

### Money

Store money exactly.

Prefer:

- integer minor units plus currency
- exact decimal with explicit scale when the domain needs variable precision

Do not store canonical money in binary floating point.

### Time

Store instants canonically as UTC or explicit epoch fields with units in the name.

For wall-clock semantics such as schedules, legal deadlines, or local dates, store the local value plus the IANA time zone when future interpretation depends on civil time.

### Identifiers

IDs may include prefixes for interoperability, but business logic should not parse identifier strings to recover hidden meaning.

### Display formats

Do not use display strings as canonical machine-interpreted storage.

### Numeric semantics

Flag:

- binary floating point for exact-decimal domains
- unchecked overflow or underflow where wrapping breaks correctness
- NaN or Infinity flowing through business logic
- implicit rounding where the rounding mode matters
- mixed units in one calculation
- exact equality on floats after approximate math

Require:

- numeric types that match precision and range
- explicit units and scale
- explicit rounding mode where rounding occurs
- central unit conversion
- tolerance-based float comparison only when approximation is part of the contract

## Temporary exceptions

A temporary representation hack is acceptable only if:

- the ambiguity is documented
- business logic stays behind a typed canonical layer
- unknown values remain safe
- visibility exists
- removal is planned

## Accept only if

- each field has one stable meaning
- high-risk concepts use explicit types or equivalent invariant checks
- internal logic consumes validated values
- unknown external values are preserved or surfaced safely
- authoritative and canonical forms are clear
- overlap during transitions has explicit precedence and mismatch handling
- numeric behavior matches the domain