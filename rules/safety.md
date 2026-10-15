# Safety Rules

Hard constraints that apply in every session, regardless of other instructions.

## File system

- **Do NOT delete files that were not created during the current session.** If a file exists on disk before the conversation started, deletion requires explicit user confirmation even if asked to "clean up".
- **Do NOT modify files outside the active project directory** unless the user explicitly names a path outside it.
- **Do NOT delete or overwrite** `node_modules/`, `.env`, `.env.*`, `*.pem`, `*.key`, `*.p12`, or any secrets/config files — confirm first.
- **Do NOT modify system-level files** — `/etc/hosts`, `/etc/sudoers`, launchd plists, systemd units, shell rc files (`~/.zshrc`, `~/.bashrc`) — without explicit per-message approval.

## Git

- **Always ask before running destructive git operations:** `reset --hard`, `clean -f`, `push --force`, `branch -D`, `rebase --onto`, `checkout -- .`, `restore .`. Typing the command is not confirmation — the user must explicitly approve the action for that specific invocation.
- **Do NOT push to any remote repository** without explicit confirmation in the current turn. A past approval does not carry over to future turns.

## Packages and global installs

- **Do NOT install global packages** (`npm install -g`, `yarn global add`, `pip install --user` or without a venv, `brew install`) without asking. Local installs scoped to the project are fine.

## Secrets and credentials

- **Do NOT print full secrets, tokens, or credentials** in responses. If a value must be referenced, show only the last 4 characters (e.g. `...f3a2`). Confirm intent first.
- **Do NOT embed secrets in generated code** — always use environment variables or a secrets manager reference.

## Database operations

- **Do NOT run destructive database operations** (`DROP TABLE`, `DELETE FROM`, `TRUNCATE`, `DROP DATABASE`) on any environment other than a local dev database (`localhost` / `127.0.0.1` with default dev credentials).
- **Always require explicit per-turn approval** before running any destructive SQL even against localhost.
- Remote databases are off-limits for destructive operations — ask the user to run them directly.

## Scope creep

- **Do NOT make changes beyond the stated task.** If a fix requires touching more files than described, surface the scope change and get approval before proceeding.
