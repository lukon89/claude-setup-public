# Verify Before Stating

Never state the status of a PR, branch, deployment, ticket, or any external resource from memory or inference. Always verify explicitly before answering.

## Required checks (examples)

| Claim | How to verify |
|---|---|
| "PR is open / merged / closed" | `gh pr list` / `gh pr view` / `git log main \| grep <ticket>` |
| "Branch exists on remote" | `git ls-remote origin <branch>` or `git branch -r \| grep <name>` |
| "Commit is on main" | `git log main \| grep <sha or ticket>` |
| "Ticket is in status X" | Fetch the ticket via API |

## Why

Stating that a PR was "still open and awaiting merge" without checking, when it had already been merged, wastes the user's time and erodes trust.

## How to apply

Before saying anything about the state of a PR, branch, merge, or deployment:
1. Run the relevant git or API command to confirm the current state.
2. Only then state the result — as a fact from the check, not an inference.

If a check is not possible (no access, no network), say so explicitly rather than guessing.
