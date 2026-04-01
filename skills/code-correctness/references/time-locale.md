---
description: Time, timezone, locale, parsing, formatting, collation, and date-storage review.
---

# Time & Locale

Use this lens for timestamps, schedules, time zones, DST, date parsing, locale-sensitive formatting, collation, Unicode normalization, and date-like strings.

## Core rules

### 1. Time zone, locale, and collation are inputs

Do not rely on host defaults when correctness depends on interpretation.

Require:

- explicit `time_zone`
- explicit `locale` where parsing or formatting depends on it
- explicit collation where string ordering or comparison depends on it
- explicit server baseline, usually UTC, unless the domain requires another zone

For future civil-time behavior, store IANA time zone IDs. Do not treat abbreviations or offsets as a substitute.

### 2. Store meaning, not display strings

Use typed canonical storage:

- instants as UTC or explicit epoch fields with units
- local schedule values as at least `(local_datetime, time_zone_id)`
- date-only concepts as date-only fields

Do not use display strings as canonical time storage.

Database, session, and driver timezone behavior must be explicit and verified.

### 3. Transport must distinguish instants from local schedules

For instants, use ISO 8601 / RFC 3339 with `Z` or an explicit offset.

For future local schedules, transport a local date/time plus an IANA time zone ID.

An offset-only timestamp is not enough to preserve future local intent.

### 4. DST creates gaps and folds

Any local-time scheduling rule must say what to do with:

- nonexistent local times
- duplicated local times

Common policies:

- strict
- shift-forward
- prefer-earlier
- prefer-later
- keep-instant

### 5. Calendar arithmetic is not duration arithmetic

Use:

- durations for elapsed time, deadlines, retry windows, and SLAs
- calendar arithmetic for human schedules

Do not assume all days are 24 hours or all months are 30 days.

### 6. Day boundaries need an authoritative zone

For day-based features, define the zone that owns the day boundary.

Compute grouping and cutoff logic in that zone.

### 7. Parsing and formatting need a locale contract

Storage and machine protocols should use unambiguous formats.

Localized human input needs an explicit locale rule.

In critical flows:

- reject ambiguous formats instead of guessing
- preserve raw input
- preserve parsed value
- preserve parse status and error class

If numbers travel as strings in a canonical protocol:

- use ASCII digits
- use `.` as decimal separator
- do not use locale-specific grouping
- define exponent support
- use exact-decimal types for money and other exact-decimal domains

### 8. Collation, equality, normalization, and identity are separate concerns

For display ordering, use locale-aware collation appropriate to the product or user locale.

For identifiers, define normalization and comparison rules per identifier type.

Do not use locale-sensitive collation for identity or security checks.

### 9. Date-as-string storage is risky

If string storage is unavoidable:

- specify the format strictly
- validate it strictly
- preserve raw value, parsed value, parse status, and failure reason during migration or import

### 10. Use the right clock

Use:

- monotonic clock for elapsed time, deadlines, timeouts, and retry windows inside a runtime
- wall clock for logs, user-visible timestamps, and calendar rules

Monotonic values are not portable timestamps.

If the domain needs one authoritative "now", define who owns it.

### 11. Future schedules need a tzdb policy

If the system stores future local schedules, define whether it:

- follows the latest civil rules
- freezes the interpretation baseline
- materializes future instances

The policy must say what is stored, when reinterpretation happens, and whether wall-clock intent or previously resolved instants win.

### 12. Field names should encode meaning

Examples:

- `occurred_at_utc`
- `expires_at_ms`
- `scheduled_local_datetime`
- `scheduled_time_zone_id`
- `effective_date`

Contracts should state:

- format
- units
- precision
- instant vs local date/time vs date-only
- whether a zone must be an IANA ID
- DST policy when relevant
- locale requirements for parsing, if localized parsing is allowed

### 13. Record interpretation context when it matters

Useful context:

- effective time zone
- locale
- collation
- tzdb version
- ICU version
- DB session timezone

On parse failures, record enough privacy-safe evidence to debug the issue.

Useful error classes:

- `invalid_format`
- `ambiguous_local_time`
- `nonexistent_local_time`
- `unknown_timezone`
- `unsupported_locale`
- `unit_mismatch`

### 14. Tests must pin time and locale

Tests should control:

- current time
- time zone
- locale
- collation where relevant
- DB or session time zone where relevant

Include boundary cases:

- DST start and end
- ambiguous and nonexistent local times
- leap day
- month-end rollover
- invalid and ambiguous user input
- conversion boundaries across driver and database layers

## Accept only if

- instants are stored canonically with explicit units
- date-only values are stored as date-only values
- local schedules preserve local intent with zone data
- DST gaps and folds are handled explicitly
- machine protocols use unambiguous formats
- localized parsing uses an explicit locale contract
- identity checks do not depend on locale-sensitive comparison
- tests control time, timezone, and locale