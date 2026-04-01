---
description: Security review for trust boundaries, authn/authz, secrets, transport security, browser boundaries, and service identity.
---

# Security & Trust Boundaries

Use this lens when code or config touches authentication, authorization, TLS, CORS, secrets, service-to-service calls, admin paths, or browser-facing security controls.

Related lenses:

- [Build Reproducibility](./build-reproducibility.md)
- [Silent Failure](./silent-failure.md)

## Review points

### 1. Treat every boundary as hostile until verified

At every trust boundary:

- authenticate the caller
- authorize the action
- document who may call what

Do not trust network location alone.

### 2. UI checks do not count as authorization

Require server-side checks for every privileged operation.

Also require:

- object-level authorization
- principal identity from authenticated context, not client-supplied identity fields
- separate server-side authorization for tenant, user, or object references provided by the client

Direct endpoint access without the UI should still be denied unless authorized.

### 3. Keep TLS verification on

If the peer is not verified, the peer identity is unknown.

Require:

- certificate validation
- hostname verification
- correct trust roots
- strong service identity; mTLS where appropriate

Any exception must be limited to local development and technically impossible in production.

### 4. CORS is not access control

Rules:

- APIs must remain secure even if CORS is misconfigured
- credentialed CORS must use a strict origin allowlist
- do not reflect arbitrary origins
- if cookies are used, provide CSRF defense
- if bearer tokens are reachable from JavaScript, treat XSS as token theft risk

### 5. Fail closed

Authentication, authorization, and policy failures should deny by default.

Do not let:

- policy-evaluation failure become implicit allow
- cached decisions expand access beyond prior authorization
- outage behavior widen access

### 6. Secrets must not leak into code or logs

Require:

- no secrets in source, tests, images, or client bundles
- runtime injection from a secret manager
- least-privilege credentials
- rotation
- no secret logging

### 7. Internal networks are not trust boundaries

Require:

- verified service identity
- no trust in spoofable headers unless a trusted proxy injects and protects them
- SSRF hardening, including metadata endpoints and other ambient credential sources

### 8. Boundary checks do not replace normal app security

Still review for:

- injection
- XSS
- unsafe uploads
- unsafe deserialization
- open redirects
- SSRF
- rate-limit or abuse gaps

## Bypass policy

A security bypass is acceptable only if:

- the bypassed control is documented
- production use is technically prevented
- every use is auditable
- every use alerts
- the bypass has an expiry or removal condition

## Accept only if

- the trust boundary is explicit
- authentication and authorization are server-side, per request, and per object where needed
- TLS verification has no production downgrade path
- CORS is strict and not treated as access control
- CSRF is covered when cookies authenticate requests
- secrets stay out of code, logs, tests, images, and client bundles
- auth and policy failures deny by default
- bypasses are non-production, audited, alerted, and time-bounded