# Pre-PR Checks

Before creating a PR (and before committing), always run the local linter and type-checker to catch failures that would fail CI.

## Required steps

```bash
make format        # or equivalent: ruff check --fix + ruff format / eslint --fix / prettier
make typecheck     # or: poetry run pyright / tsc --noEmit / mypy
make test          # full test suite or the relevant subset
```

Run all three before the first commit on a branch. Re-run after any non-trivial change.

## Lock file discipline

**Never manually edit lock files** (`poetry.lock`, `package-lock.json`, `yarn.lock`) — they are generated files.

Whenever dependency constraints change (version bumps, additions, removals):

```bash
# Python
poetry update <package>   # update one package and regenerate lock
# or
poetry lock               # regenerate entire lock from pyproject.toml

# Node
npm install               # regenerates package-lock.json
```

CI runs `install` against the lock file and fails immediately if they are out of sync. There is no way to fix this from CI — the lock file must be regenerated locally.

## Resolving lock file merge/rebase conflicts

**Never resolve lock file conflicts as text.** The file is machine-generated — text-merging produces a corrupt result. Instead:

1. Accept one side wholesale:
   ```bash
   git checkout --theirs poetry.lock   # during rebase
   git add poetry.lock
   git rebase --continue
   ```
2. Regenerate from the manifest file.
3. `git add` and commit/push.

## Why

A pipeline failure after pushing forces a fix commit that wouldn't have been needed if checks had been run locally first. This wastes CI minutes and creates noise in the PR history.

## How to apply

- Before `git commit` on a new branch: run format and typecheck at minimum.
- Before `gh pr create`: run the full suite.
- If typecheck surfaces errors unrelated to the current change (pre-existing), note them but do not fix them unless asked — ensure no **new** errors were introduced.
