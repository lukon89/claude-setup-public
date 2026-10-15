# Migration Discipline

All DB schema changes introduced in a single PR must live in a single migration file. If a schema decision changes mid-PR (e.g. a column is added, then a reviewer asks for it to be removed), edit the existing unmerged migration directly — do not create a new migration on top.

**Why:** Unmerged migrations have never run in production. A second migration that undoes the first creates unnecessary history, makes rollbacks harder to reason about, and leaves dead migrations in the repo. The clean invariant is: one logical change = one migration file per PR.

**How to apply:** When a PR review requires a schema change to an existing unmerged migration:
1. Edit the migration file directly (add/remove columns, indexes, etc.)
2. Drop the affected table locally and re-run from scratch to validate
3. Never run `aerich migrate` / `alembic revision` / `rails generate migration` to layer a fixup on top of an unmerged migration

This only applies to migrations that have not yet been merged and deployed to any environment. Deployed migrations are immutable.
