# Claude Code — Custom Agent Setup

A collection of rules, skills, and configuration for Claude Code that enforces team conventions and automates common developer workflows.

## What's here

```
rules/      Global rules loaded into every Claude Code session
skills/     Custom slash commands (/create-pr, /analyse-pr-comments, etc.)
CLAUDE.md   Example project-level Claude configuration
```

## How it works

Rules live in `~/.claude/rules/` and are referenced from `~/.claude/CLAUDE.md` via `@` imports. They load automatically in every session — no manual invocation needed.

Skills live in `~/.claude/commands/` and are invoked as `/skill-name` in the Claude Code prompt.

## Rule Factory

Patterns are first logged in `Learnings.md` as *incubating*. After a second occurrence across conversations, a rule is proposed and added to `rules/`. This keeps rules grounded in real observations rather than hypothetical conventions.

See `rules/rule-factory.md` for the full process.

## Note on commit history

The rules and skills in this repo were extracted from my private Claude Code configuration that I've been building over several months. The initial commit date doesn't reflect when these patterns were developed — it reflects when I pulled the non-company-specific ones into this public repo.

## Setup

```bash
# Clone into your Claude config directory
git clone <this-repo> ~/.claude/setup-public

# Reference rules from your ~/.claude/CLAUDE.md:
# @~/.claude/setup-public/rules/safety.md
# @~/.claude/setup-public/rules/coding-guidelines.md
# ... etc

# Copy skills to commands directory
cp skills/*.md ~/.claude/commands/
```
