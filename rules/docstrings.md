# Docstrings

Add a short docstring to any function or method whose purpose, inputs→outputs, or side effects are not immediately obvious from its name and signature. Skip trivially self-explanatory functions.

## When to add one

Add a docstring when the function has any of:
- **Non-obvious transformation** — parsing/mapping between shapes (e.g. raw API dict → internal model). Say *from what, to what*, and name the model if it's a custom projection.
- **Meaningful length or branching** — multi-step orchestration, several `try`/`except` arms, non-trivial control flow. Summarise the flow, not line-by-line.
- **Side effects worth flagging** — writes to DB / external services, or notably the *absence* of them ("Pure read — nothing is persisted").
- **Error/edge behaviour** — what it raises or how failures surface.
- **Domain rules** — matching logic, precedence/priority order, invariants a reader can't infer from names.

## When NOT to add one

- Trivial helpers whose name says everything: `add_numbers(numbers: list[int]) -> int`, `is_empty(x)`, simple getters/passthroughs.
- Test functions (the test name is the description).
- Don't restate the signature in prose ("Takes a list and returns an int" adds nothing).

## Style

- One-line summary first; add a short body paragraph only if there's real nuance (flow, side effects, errors).
- Explain *why* / *what contract*, not *how* line-by-line — the code shows how.
- Keep it current: if behaviour changes, update the docstring in the same edit.
- **Truth tables must include all guard conditions** — every row must list all conditions that determine that outcome. A table that omits a guard implies the simpler condition alone determines the outcome, which misleads the reader.
