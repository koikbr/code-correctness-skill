---
description: Storage and rollout migration review for backfills, online schema changes, cutovers, and partial completion under live traffic.
---

# Online Migrations

Use this lens for database schema changes, backfills, reindexing, type changes, large data moves, and live cutovers.

Related lenses:

- [Schema Evolution](./schema-evolution.md)
- [Reliability](./reliability.md)

## Review points

### 1. Rollback is not undo

Assume database changes are forward-only unless reversibility is proven.

Good planning includes:

- safe stop points
- roll-forward plan
- deferred destructive work
- read-path or feature gating that can revert safely
- tested backup and restore for disaster recovery, not as the normal migration path

### 2. Online work must be bounded

Red flags:

- unbounded full-table updates
- long transactions
- disruptive locks under traffic
- index creation without the required online or concurrent mode
- backfills competing with serving traffic with no throttle

Require:

- bounded batch size and batch time
- health-based throttling
- pause and abort conditions
- idempotent and resumable jobs

### 3. Backfills must handle concurrent writes

A backfill is not done when the script exits.

Require:

- idempotent batches
- persistent checkpoints
- resumability
- bounded work per batch
- rate limiting or adaptive throttling
- metrics for rows processed, errors, retries, lag, and ETA
- validation by invariants, counts, samples, or checksums
- an explicit design for writes that land during the backfill window

### 4. Migration state must be explicit

Track durable, monotonic gates such as:

- schema expanded everywhere
- backfill running and verified
- concurrent writes reconciled
- read cutover staged and monitored
- write cutover staged and monitored
- old path disabled
- contract completed and verified

Require:

- durable migration state
- monotonic transitions
- automated gates between phases
- runbook with prechecks, monitors, abort thresholds, and roll-forward steps

### 5. Enforce invariants where the database can help

Prefer database enforcement when feasible:

- `NOT NULL`
- `UNIQUE`
- foreign keys
- `CHECK`

Validate existing data before tightening constraints.

Triggers are a last resort. If used, require:

- performance testing
- lock-impact testing
- observability
- removal condition

### 6. Renames are breaking under skew

If old and new code may coexist, do not rename in place.

Prefer:

1. add new representation
2. migrate readers
3. migrate writers
4. verify old readers and writers are gone
5. remove the old representation

### 7. Cutover must follow production evidence

Do not contract based on:

- code search alone
- assumptions about deploy order
- assumptions that old clients are gone
- assumptions that backfill "must have finished by now"

Use telemetry and migration state.

## Minimal procedure

1. Expand without breaking old code.
2. Run the migration with bounded, resumable work.
3. Capture and reconcile concurrent writes.
4. Cut over reads and writes in stages.
5. Contract only after evidence shows the old path is unused.

## Temporary exceptions

A risky migration workaround is acceptable only if it states:

- the risk
- hard bounds: throttle, timeout, batch cap, abort threshold
- evidence to watch
- safe stop point
- roll-forward plan
- cleanup owner
- removal condition

## Accept only if

- online work is bounded, throttled, idempotent, and resumable
- lock behavior, transaction time, and replication impact are understood
- concurrent writes during migration are captured or reconciled
- migration state is explicit and durable
- cutover is based on telemetry
- destructive cleanup waits until production evidence says it is safe