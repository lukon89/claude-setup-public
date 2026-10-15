# Rules Index

Format: `[rule-name] | created: YYYY-MM-DD | one-line summary`

## Safety & correctness
- [safety] | created: 2026-10-13 | hard constraints: file deletion, destructive git, secrets, DB operations, scope creep
- [verify-before-stating] | created: 2026-10-13 | verify PR/branch/ticket state via API before stating it — never from memory
- [verify-in-source-of-truth] | created: 2026-10-13 | only a direct DB query confirms data exists — not logs or analytics pipelines

## Workflow
- [branch-from-main] | created: 2026-10-13 | always fetch + pull before creating a branch; never branch from stale local state
- [pre-pr-checks] | created: 2026-10-13 | run format + typecheck + tests before committing; never manually edit lock files
- [migration-discipline] | created: 2026-10-13 | one migration per PR; edit unmerged migrations directly, never stack fixups
- [reply-to-pr-comments] | created: 2026-10-13 | after fixing a PR comment, always reply to that specific thread in the same turn
- [investigate-precisely] | created: 2026-10-13 | extract exact timestamps from alert source before querying logs; never guess time ranges

## Coding style
- [coding-guidelines] | created: 2026-10-13 | living doc: named constants over inline magic values
- [frontend-coding-guidelines] | created: 2026-10-13 | living doc: named helpers, JSDoc at default values, explicit generic types, role-based test selectors
- [docstrings] | created: 2026-10-13 | add docstrings to non-trivial functions; skip obvious one-liners; never restate the signature
- [explicit-query-types] | created: 2026-10-13 | keep explicit type annotations on generic wrappers even when compiler infers
- [backend-code-scope] | created: 2026-10-13 | no UI component names or screen labels in backend code/docs
- [no-ticket-numbers-in-filenames] | created: 2026-10-13 | name files by what they do, not which ticket created them
- [collateral-fix-discipline] | created: 2026-10-13 | when fixing collateral breakage, read the file's tests first and preserve observable behaviour
- [implementation-completeness-checklist] | created: 2026-10-13 | before PR: check failure-path logging, test coverage, I/O ordering, schema type parity

## Communication
- [copy-paste-format] | created: 2026-10-13 | wrap copy-paste output in plain triple-backtick block, no markdown inside
- [slack-draft-only] | created: 2026-10-13 | always draft Slack messages; never post directly without explicit per-turn approval
- [interactive-questions] | created: 2026-10-13 | ask clarifying questions one at a time; wait for answer before the next

## Meta
- [rule-factory] | created: 2026-10-13 | incubate patterns in Learnings.md; propose rules after second occurrence
- [preserve-venv] | created: 2026-10-13 | never recreate the Python venv just to run checks; ask before recreating
