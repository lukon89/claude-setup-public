# Implementation Completeness Checklist

Before treating a feature implementation as done — and again during code review — check these four dimensions explicitly.

## 1. Debug-worthy logging

Every path where a request is rejected, a conflict is detected, or a retry/fallback branch fires must log with enough structured context (ids, not just a message) to diagnose the event without reproducing it locally:
- Authorization/permission rejections (who was rejected and why)
- Business-logic conflicts (ownership mismatches, duplicate-key races, constraint violations)
- Branches that handle an unusual-but-expected case (out-of-order events, concurrent-write races)
- Distinguish "created new" vs "returned existing" on idempotent/upsert operations

A bare `logger.info(...)` at the top of a route for the request shape is not enough — the interesting information is on the failure/branch paths.

## 2. Test scenario completeness

Beyond the golden path, check for:
- **Duplicate/repeat calls with drifted data** — if the spec says "return existing on conflict", test a repeat call with *changed* field values and assert the original data is untouched.
- **Cross-tenant/ownership edge cases** — verify what happens when a different tenant references the same key.
- **Downstream/dependency failures** — test what happens when an external API call errors, times out, or returns an unexpected shape.
- **Constraint/validation edges** — oversized strings, out-of-range decimals, wrong-length codes. The request schema should reject these with a clear 422, not a persistence-layer failure.
- **Concurrent/race conditions** — a try/except around IntegrityError is a claim; verify it with a test where the constraint fires.
- **Guard/boundary conditions on new branches** — when a new code path is guarded by a boolean condition, add a test where the guard is false and verify the old behaviour is preserved.
- **Assertion precision** — if the test name makes a specific claim (e.g. "first item wins"), the assertion must pin exactly that, not a weaker property.

## 3. Validation and cheap computation before I/O

In consumer action functions and service methods that mix validation with DB/network calls, all input validation and pure computation must complete **before** the first DB session / HTTP call:
- If a function validates a payload field's shape or value, that check must happen before any DB lookup.
- If a function derives a value from the payload, that derivation must happen before any I/O.

**Why:** If validation is placed *after* a DB call, every test for that validation path must mock the DB — even when the DB is irrelevant to what's being tested.

**How to apply:** Check that the function's control flow is: validate inputs → compute from inputs → open session → do I/O. If I/O precedes validation, move the validation earlier.

## 4. Pydantic/DB schema typing parity

For every Pydantic request/response model backed by a DB model:
- String fields must mirror the DB column's `max_length` via `Field(max_length=...)`, not accept unbounded strings.
- Decimal fields must mirror `max_digits`/`decimal_places`.
- Fields with a known, closed value set should be typed as `Literal[...]` or an enum — but verify the set is genuinely closed before choosing strict `Literal` over a pattern-constrained `str`.

## How to apply

When reporting an implementation as complete, or when reviewing a diff, explicitly walk through these four items before declaring done. If a dimension doesn't apply (e.g. no external API calls), say so explicitly rather than silently skipping it.
