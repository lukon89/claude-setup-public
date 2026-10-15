# Rule Factory

During our work together, watch for repeated patterns in:
- code review preferences (naming, structure, comments)
- refactoring decisions (what to extract, what to inline)
- bug analysis approaches (how to diagnose, what to check first)
- naming conventions (variables, functions, files, branches)
- error handling style (how to structure try/catch, what to log)
- comment & documentation habits (when to add them, how verbose)
- test writing patterns (unit vs integration preference, what to always assert)
- dependency decisions (what libraries to reach for vs avoid)
- git habits (commit messages, branch naming, squash vs merge)
- debugging approach (logging first, reproducing steps, isolation strategy)
- preferred response format (code first vs explanation first, brief vs detailed)
- security patterns consistently applied
- framework-specific conventions
- trade-offs consistently made (readability vs performance, etc.)
- how ambiguity is handled (ask first vs assume and note it)

## Incubation via Learnings.md

If a pattern appears **once**, log it to `Learnings.md` rather than proposing a rule immediately:
- Cross-project soft pattern → `~/.claude/Learnings.md`
- Codebase-specific discovery → `.claude/Learnings.md` (local to the current project — create if absent)

When the same pattern appears a **second time**, check Learnings.md first. If it's already logged, that's the trigger to propose a rule.

A pattern is worth proposing when you have seen it at least **twice**, or recognised it as consistent with a previously saved rule.

When you spot a pattern worth codifying, first check `~/.claude/rules/_index.md`.
Then ask one of:
> "I noticed [specific pattern]. Should I create a new rule for this?"
> "I noticed [specific pattern]. Should I extend [existing-rule-name] with this?"

If creating new, ask:
> "Should this be global (all projects) or local (this project only)?"

Then either:
- **Create** → `~/.claude/rules/<rule-name>.md` (global) or `.claude/rules/<rule-name>.md` (local)
- **Extend** → append the new pattern to the existing rule file

After any change, update `~/.claude/rules/_index.md`:
```
- [rule-name] | global/local | created: YYYY-MM-DD | updated: YYYY-MM-DD | one-line summary
```
