---
description: Contract evolution review for API, event, schema, and format changes under version skew.
---

# Schema Evolution

Use this lens when changing APIs, events, messages, file formats, or any published contract that old and new code may need to read or write at the same time.

Related lenses:

- [Data Representation](./data-representation.md)
- [Online Migrations](./online-migrations.md)

## Review points

### 1. State the compatibility target

For each change, name the required safe reader/writer pairs.

Examples:

- old readers can read new writes
- new readers can read old writes

Do not hide behind generic compatibility labels alone.

Each API surface should use one versioning strategy:

- path versioning
- header or content-type versioning
- schema ID in payload or envelope
- additive-only versionless evolution

For each supported version, define support status, removal policy, and discovery path.

### 2. Additive first

Default plan:

1. add the new field, endpoint, event, or variant
2. teach consumers to handle old and new forms
3. deprecate the old form with visible signals
4. remove only after usage evidence says it is safe

Do not rename or remove published fields in one step when skew can exist.

### 3. Semantic changes are breaking changes

Shape staying the same does not make the change safe.

Examples:

- `status` gains a new value older clients do not handle
- `amount` changes from cents to dollars
- `timestamp` changes from UTC to local time
- a boolean meaning flips

If meaning changes, use a new field, new event type, new version, or new representation.

### 4. Unknown and optional data need explicit handling

Consumers should tolerate unknown fields and missing optional fields where the format allows it.

Unknown values must not be collapsed into a normal business value.

If round-trip fidelity matters, preserve unknown fields where the format/runtime allows it, or provide an opaque passthrough map.

### 5. Deprecation is not done until it is measured

A deprecated field, endpoint, or event needs:

- documented support policy
- measurable usage
- caller-visible migration signal where practical
- removal condition or long-term-support decision

"Nobody uses this" is not evidence.

Useful checks:

- consumer-driven contract tests
- provider-side compatibility tests against locked fixtures
- schema-registry validation in CI
- golden vectors tied to a published baseline

### 6. Events are published APIs

Event contracts need:

- explicit versioning or schema identity
- policy for unknown event types
- stable meaning
- compatibility validation in CI and release review

### 7. Assume mixed versions

Skew, lag, stale readers, and stale writers are normal.

Flag plans that assume:

- synchronized rollout
- immediate convergence
- instant backfill completion
- consumers updating before the provider ships a breaking change

### 8. Use expand, migrate, contract

When old and new code coexist:

- **expand**: add the new surface in a form old code tolerates
- **migrate**: keep both forms valid while reads and writes transition
- **contract**: remove the old path only after evidence says it is unused

Before contract, require:

- expand complete everywhere that can serve traffic
- overlap verified
- cutovers staged and monitored
- telemetry evidence, not code search alone

### 9. Format-specific checks

#### Protocol Buffers / gRPC

- never reuse field numbers
- never change a field's wire type
- reserve removed numbers and names
- treat enum growth as a compatibility event
- prefer additive changes

#### JSON / REST

- do not remove or rename fields without deprecation
- new response fields should be optional for existing clients
- new request fields must not become mandatory for existing clients unless safely defaulted or versioned
- define `null`, absent, and empty semantics
- do not change units, timezone rules, or identifier meaning in place

#### Avro / schema registry

- register and validate schemas before publishing
- state compatibility in reader/writer terms
- provide defaults where needed
- do not treat the registry mode as a substitute for semantic review

#### Events / messages

- include explicit schema identity or equivalent
- define policy for unknown event types
- keep event meaning stable
- do not silently drop fields in transformation pipelines when downstream compatibility depends on them

## Temporary exceptions

If a breaking change is unavoidable, require:

- what breaks
- who is affected
- migration path
- bounded coexistence window
- visibility
- exit plan

## Accept only if

- required reader/writer pairs stay safe during rollout and support
- the change is additive first or properly versioned
- semantic changes use new fields, versions, or representations
- unknown values are handled safely
- deprecations are measured and policy-backed
- mixed versions can coexist safely
- compatibility tests cover the real boundary