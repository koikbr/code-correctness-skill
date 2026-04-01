---
description: Telemetry review for correlation, logs, metrics, traces, alerts, dashboards, privacy, and bounded cardinality.
---

# Observability Gaps

Use this lens when code changes telemetry, failure reporting, async processing, paging, dashboards, or incident workflows.

Related lenses:

- [Silent Failure](./silent-failure.md)
- [Reliability](./reliability.md)

## Review points

### 1. Instrument decisions, not decoration

Each metric, log event, trace, alert, and dashboard panel should support at least one of:

- a question
- a decision
- a diagnosis step

Useful incident questions:

- what is broken
- who is affected
- where it started
- what changed
- what to do next

### 2. Correlation is mandatory

Telemetry should preserve causal context across sync and async boundaries.

Useful identifiers:

- `trace_id`
- `span_id`
- `request_id`
- `message_id`
- `job_id`
- `run_id`
- `workflow_id`

Minimum shared fields usually include:

- `timestamp`
- `level`
- `service`
- `environment`
- compute location
- `service_version`
- `build_id` or `deploy_id`
- region
- normalized operation identity
- status code or class
- bounded change context such as config version or incident-relevant feature flags

`endpoint` should mean a normalized route or operation template, not a raw path.

### 3. Privacy is a hard limit

Do not log:

- secrets
- tokens
- auth headers
- payment data
- raw personal data
- raw request or response bodies unless justified and scrubbed

Prefer pseudonymous tenant or customer identifiers where context is needed.

### 4. Logs should be structured and stable

Require:

- structured output
- stable keys
- explicit event names
- automatic correlation fields where applicable
- error type, code, message, location, and stack when safe
- bounded volume

Do not rely on free-form text parsing for core workflows.

### 5. Metrics should support action

Required metric families often include:

- user-impact indicators such as success rate and latency compliance
- traffic, errors, and saturation on critical paths
- dependency health
- queue and concurrency pressure
- error-budget burn where SLOs apply

Metric labels must be bounded. Do not put `user_id`, `request_id`, raw paths, or arbitrary query strings in labels.

### 6. Traces are for causal debugging

At minimum, trace:

- ingress and egress
- external service calls
- database and cache operations
- queue publish and consume
- retries
- expensive compute on critical paths

Sampling must keep enough failed and slow traces to investigate incidents.

### 7. Alerts should page only on impact and action

A page should correspond to:

- real user or business impact, or strong evidence it is imminent
- a likely next action
- a drill-down path

A page should include or link to:

- what is broken
- impact size
- likely scope
- recent change context if known
- next diagnostic step

### 8. Failures must be classifiable

Avoid one giant "error" bucket.

Separate by:

- error class
- expected vs unexpected
- first-attempt vs retry outcome
- failing dependency or component
- operation or workflow step

A shared taxonomy across services helps.

### 9. Dashboards should match incident flow

Each critical service should have a primary incident dashboard with:

- SLI and SLO status
- golden signals by normalized route or operation and region
- dependency health
- deploy and release context
- config and relevant feature-flag changes
- latency distribution
- error classes
- queue lag and backlog where relevant
- saturation tied to real bottlenecks

### 10. Async systems need lag and disposition visibility

For jobs, queues, and workflows, expose:

- message or job identity
- causal linkage to triggering work
- enqueue, start, finish times
- queue depth
- oldest item age
- processing rate
- retry rate
- dead-letter or quarantine rate
- final disposition

### 11. Cardinality and cost are reliability concerns

Require:

- bounded metric labels
- bounded trace attributes
- controlled log volume
- explicit retention and sampling strategy
- budgets for telemetry volume and cardinality

Correlation IDs belong in logs and traces, not metric labels.

## Temporary exceptions

A telemetry gap is acceptable only if it states:

- the blind spot
- the operational risk
- bounds
- mitigation
- owner
- removal condition

## Accept only if

- impact and scope can be measured quickly
- causal context survives sync and async boundaries
- logs are structured, searchable, and privacy-safe
- metrics use bounded labels and support decisions
- traces cover the critical path
- alerts point to action
- dashboards follow incident flow
- async lag and final disposition are visible where relevant
- telemetry cost is bounded