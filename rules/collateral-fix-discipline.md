# Collateral Fix Discipline

When an auto-merge or dependency/tooling change introduces a breakage in a file unrelated to the current PR, fixing it is required — but the fix must be complete and verified, not just "unblocking".

## The pattern to avoid

1. An external change (auto-merge, dependency bump, linter upgrade) breaks file X
2. Quick fix makes the error go away
3. The fix silently removes or alters behaviour that existing tests relied on
4. CI goes red in an unrelated area
5. A reviewer asks: "why are these changes here?"

## Required behaviour

When a collateral fix is needed:

1. **Read the tests for the file being touched** before making the change. Understand what the existing behaviour is — don't just make the error go away.
2. **Preserve observable behaviour** — if the old code produced a specific output or side effect, the replacement must too.
3. **Run the relevant tests locally** before committing.
4. **In the PR, explain the collateral change** via a commit message or comment so reviewers aren't confused by it appearing in an otherwise unrelated diff.

## How to apply

Any time you touch a file outside the PR's stated scope:
- Ask: "does this file have tests? What do they assert about the current behaviour?"
- Read those tests before editing
- Verify the fix preserves what the tests expect
- If the fix genuinely changes observable behaviour, flag it explicitly
