---
description: Toggle and config review for rollout flags, experiments, kill switches, precedence, evaluation timing, and cleanup.
---

# Flag Debt

Use this lens when behavior is controlled by flags, runtime config, tenant overrides, experiments, kill switches, or migration switches.

Related lenses:

- [Global State](./global-state.md)
- [Online Migrations](./online-migrations.md)

## Review points

### 1. Pick the narrowest control that matches the job

Common control types:

- release flag
- kill switch
- experiment flag
- durable entitlement or capability
- operational config
- migration bridge flag

Do not leave rollout scaffolding in place as the permanent product mechanism.

### 2. Define runtime semantics

Each control needs:

- type
- default
- scope
- precedence order
- evaluation model
- outage behavior

Defaults should be intentional and safe.

If values can change at runtime, be clear about whether mid-request change is allowed.

Experiments that affect user-visible behavior usually need sticky assignment.

### 3. Keep the state space small enough to reason about

Prefer one mode enum such as `off | shadow | on` over several booleans.

Watch for:

- nested conditionals combining flags, env vars, tenant overrides, and defaults
- bugs that appear only under rare combinations

If more than a few controls shape one behavior, require a design note for the allowed state space.

### 4. Validate config before it changes behavior

Require:

- schema or type validation
- numeric bounds
- enum validation for closed sets
- cross-field validation where fields interact
- staged rollout and rollback for risky changes

### 5. Telemetry should explain active behavior

Emit enough signal to tell:

- disabled
- enabled
- partially enabled
- degraded
- enabled for only some scope

Keep telemetry bounded. Do not explode cardinality just to explain flag state.

### 6. Persisted-data transitions need an end state

For migration bridge flags, define:

- target steady state
- read behavior during overlap
- write behavior during overlap
- completion criteria
- cleanup owner

Do not leave stored data split across formats by accident.

### 7. Release flags are temporary

Temporary rollout flags need a removal condition.

Durable config and entitlements need a review condition even if they are meant to stay.

### 8. Kill switches must reduce risk

A kill switch needs:

- clear purpose
- blast radius
- owner
- tested activation path
- verification that activation does not trigger secondary failures such as retry storms or fallback overload

## Lifecycle

Use this sequence for temporary rollout flags:

1. proposed
2. implemented
3. rolling out
4. locked for a short confirmation window
5. removed

## Temporary exceptions

A flag or config exception is acceptable only if:

- type and purpose are declared
- an owner is named
- a review or removal condition exists
- defaults and outage behavior are documented
- observability is sufficient to explain effects
- cleanup is enforceable

## Accept only if

- each control has a type, owner, lifecycle state, and review or removal condition
- default, scope, precedence, evaluation timing, and outage behavior are explicit
- unsupported combinations are rejected, guarded, or tested
- validation prevents unsafe values
- behavior can be explained from telemetry
- migration bridge flags have completion criteria and cleanup ownership
- kill switches are exercised and do not create new failure modes