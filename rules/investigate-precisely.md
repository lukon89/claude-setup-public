# Investigate Precisely

When investigating a bug, alert, or error report, never guess at time ranges or search broadly. Always extract the exact timestamp from the source (Sentry, PagerDuty, alert, etc.) before querying logs or metrics.

## Required steps before querying any log system

1. **Read the source first.** If given an alert link or ticket — fetch it via API and extract: error message, culprit, first seen, last seen, and event count.
2. **Use exact timestamps.** Convert first/last seen to the correct Unix epoch for the target system. Verify the year — a copy-paste or off-by-one error silently queries the wrong year and returns empty results with no warning.
3. **Target the right log group.** The culprit field names the exact service/function. Query that log group, not a broad sweep.
4. **Fetch the latest event.** For issue trackers, get the full stack trace and structured fields before opening the log system — this avoids one round-trip of guessing which log stream to read.

## What not to do

- Do not widen the time window "to be safe" before checking the exact window first.
- Do not scan multiple log groups for errors when the culprit is already known.
- Do not ask the user for details (error type, timestamp, service name) that are available in the linked source — fetch them directly.

**Why:** Guessing time ranges and log groups wastes time and produces noise. The source of truth always contains the precise coordinates needed to go straight to the right log stream at the right time.
