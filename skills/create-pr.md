# /create-pr

Create a PR for the current work. Enforces quality gates before pushing and produces a consistent PR format.

## Before anything else — quality gates

Run these in parallel and **stop if any fails**:

```bash
make format    # auto-fix what can be fixed
make typecheck # must exit 0 — no new errors allowed
```

If `make` targets don't exist, use the repo's equivalent (`ruff check --fix`, `tsc --noEmit`, etc.). Ask the user what to run if unclear — don't skip.

Run tests only if the change touches logic (not config, docs, or types-only changes):
```bash
make test
```

Do not proceed to git steps until all gates pass.

## Git steps

```bash
git fetch origin
git status        # confirm working tree state
git diff HEAD     # review what's changing
git log --oneline -5  # learn commit style of this repo
```

If not already on a feature branch, create one now:
```bash
git checkout -b <type>/<short-description>
```
`<type>`: `feat` for new functionality, `fix` for bugs, `chore` for non-functional changes.  
`<short-description>`: 3–5 words, kebab-case.

Stage only files relevant to this change:
```bash
git add <specific files>   # never git add -A without reviewing first
```

## Commit

Title: imperative mood, ≤72 chars. Body: explain *why*, not what.

```bash
git commit -m "$(cat <<'EOF'
<type>: short imperative summary

Why this change was needed and what approach was taken.
Leave out anything obvious from the diff.

Co-Authored-By: Claude <noreply@anthropic.com>
EOF
)"
```

If pre-commit hooks reformat files: stage the reformatted files, then make a **new commit** — never `--amend` after a hook run.

## Push and open PR

```bash
git push -u origin HEAD
```

Ask the user for the ticket ID/URL if not already known. If they say there's no ticket, confirm once before proceeding without it.

```bash
gh pr create \
  --title "<type>: short imperative summary" \
  --base main \
  --body "$(cat <<'EOF'
## What and why
<!-- 2–4 bullets: what changed, why it was needed, any non-obvious trade-offs -->
-
-

## How to test
<!-- What a reviewer should do to verify this works -->
-

## Ticket
<!-- Link or "none" -->

🤖 Co-authored with [Claude Code](https://claude.ai/claude-code)
EOF
)"
```

Print the PR URL at the end.

## If quality gates fail

Fix the issue, don't bypass it. If a pre-existing violation is unrelated to the current change, note it explicitly ("pre-existing, not introduced here") but do not fix it in this PR — keep the diff focused.
