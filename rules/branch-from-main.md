# Branch from Main

Before creating any new branch, always fetch and branch off the latest `main` (or the repo's default branch). Never branch from a stale local state.

**Required steps:**
```bash
git fetch origin
git checkout main
git pull origin main
git checkout -b <new-branch>
```

**Why:** Branching from an outdated local state means the new branch diverges from main by potentially dozens of commits. When a PR is eventually opened, the diff includes all those unrelated commits and files — making the PR hard to review, and risking conflicts with migrations or other files already merged.

**How to apply:** Any time the user asks to create a branch or start work on a new ticket, run `git fetch origin && git pull origin main` first, then branch from the updated main. If work is done on the wrong branch, stash, switch to main, pull, create the correct branch, and pop the stash. Independent tasks always start from the top of main unless they explicitly depend on another in-progress branch.
