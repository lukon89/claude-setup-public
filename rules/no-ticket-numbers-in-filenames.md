# No Ticket Numbers in File or Directory Names

Never use project management ticket IDs (e.g. `TICKET-766`, `ticket766`) in file names, directory names, or module names in production repositories.

## Why

Ticket numbers in file paths rot immediately:
- The ticket closes; the file stays forever with a name that means nothing to future readers.
- Git blame and the commit message already record which ticket introduced the code — embedding it in the path is redundant.
- It leaks internal project management conventions into the public codebase.

## What to do instead

Name files and directories after **what the code does**, not **why it was created**:

| Wrong | Right |
|---|---|
| `backfill_transactions_ticket766/` | `backfill_bank_feed_transactions/` |
| `fix_exchange_rates_ticket123.py` | `fix_exchange_rates.py` |
| `seed_roles_ticket456/` | `seed_entity_roles/` |

If you must reference the ticket, put it in:
- The Git commit message body
- A `# Context: TICKET-766` comment in the module docstring (acceptable but not required)
- The PR description

## How to apply

Before creating any new file, module, or directory: does the name contain a ticket ID? → rename before committing. If already committed with a bad name, rename via `git mv` in the same PR before it merges.
