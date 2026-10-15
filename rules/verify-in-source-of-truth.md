# Verify in Source of Truth

When investigating whether data exists or an operation succeeded, logs and indirect signals (analytics pipelines, API responses logged elsewhere) are **not confirmation**. The only acceptable confirmation is a direct query against the source of truth.

**Why:** Logs may describe an intent ("entity created") but not the committed outcome. Analytics pipelines add lag and can silently drop rows. Assuming data exists based on logs wastes time chasing a problem that doesn't exist — or worse, missing one that does.

**How to apply:**
- When asked "is this data in the DB?" — do not answer based on logs or any derived view. Obtain direct DB access and run the query.
- If direct DB access isn't available, say so explicitly and ask the user how to connect — don't substitute with indirect evidence.
- The standard of confirmation is: a query returned the row. Nothing less counts.
- This applies to: backfill verification, debugging missing records, confirming an event was processed, checking sync state.
