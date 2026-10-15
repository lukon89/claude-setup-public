# Preserve Virtual Environment

Never run `poetry env use <version>`, `pyenv local <version>`, or any command that recreates or switches the active virtual environment when the goal is simply to run a check (type checking, tests, linting).

**Why:** Recreating the venv forces a fresh package install that may surface pre-existing errors invisible to CI (which uses a cached venv). This can trigger a multi-PR detour to fix violations that CI was never checking, wasting significant time.

**How to apply:**
- Before running any check command, verify the existing venv is usable: `poetry env info` or `poetry run python --version`.
- If a command fails due to a missing tool, investigate which Python version owns that tool and activate it — do NOT recreate the venv.
- If the venv is genuinely broken or missing, ask the user before recreating it — explain the risk that a fresh install may expose pre-existing errors.
- Never assume a Python version mismatch means you should switch versions. Check `pyenv local` and `poetry env info` first, then ask.
