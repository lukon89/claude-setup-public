# My Daily Workflow with Claude Code

This file describes how I actually use Claude Code day-to-day — which tools, skills, and rules kick in at different stages of a typical task.

---

## MCP Servers

### Local (self-hosted)

| Server | When it's used |
|---|---|
| **GitHub** | PRs, issues, code review, branch management — Claude interacts with GitHub directly without me switching context |
| **Jira (Atlassian)** | Reading ticket details, transitioning statuses, adding comments — ticket lifecycle is automated |
| **Playwright** | Browser automation during debugging, checking rendered output, E2E verification |
| **Figma** | Design-to-code context — Claude reads component specs directly from Figma when implementing UI |

### Cloud (claude.ai integrations)

| Server | When it's used |
|---|---|
| **Slack** | Drafting code review requests, status updates — always as drafts, never posted directly |
| **Notion** | Documentation, notes, knowledge base lookups |
| **Snyk** | Security scanning triggered during PR review — SCA, IaC, container |
| **Jira (cloud)** | Backup access when local MCP is unavailable |

---

## Skills (slash commands)

### `/create-pr`
Triggered when I'm ready to ship a change. It:
1. Runs quality gates (format, typecheck) — stops if they fail
2. Creates a branch with a consistent naming convention
3. Stages only relevant files, writes a structured commit message
4. Opens a PR with a standard template (what/why, test plan, ticket link)

### `/analyse-pr-comments`
When a PR comes back with review comments. It:
1. Fetches all reviewer comments (skips my own)
2. For each comment: assesses → fixes if needed → replies to the thread
3. Checks CI after all replies — diagnoses and fixes failing jobs
4. Produces a summary table at the end

### `/slack-code-review-request`
After opening a PR. Drafts a Slack message for a specific reviewer in a specific format, waits for my approval before sending. Never posts directly.

---

## How a typical task flows

```
Morning brief (auto)
  → Claude reads yesterday's notes + active tasks + any open PRs
  → surfaces what needs attention without me asking

Pick up a ticket
  → Claude fetches ticket details from Jira
  → transitions it to In Progress automatically
  → branches from latest main

Implementation
  → rules loaded in background enforce style, logging, test patterns
  → implementation-completeness-checklist checked before declaring done

/create-pr
  → quality gates run
  → PR opened with structured description
  → Jira ticket transitions to Code Review

/slack-code-review-request
  → draft message prepared, I send it

Review comes back
  → /analyse-pr-comments
  → all comments addressed, CI green, summary produced

Merge
  → Jira ticket transitions to Ready for Prod
  → work log updated
```

---

## Rule Factory — how rules evolve

I don't write rules upfront. The process:

1. Something notable happens in a session (a correction, a confirmed approach)
2. Claude logs it in `Learnings.md` as *incubating*
3. When the same pattern appears a second time, Claude proposes a rule
4. I decide: global (all projects) or local (this project only)?
5. Rule is added to `rules/`, indexed in `rules/_index.md`

This means rules are grounded in things that actually happened, not hypothetical conventions.

---

## Work Log

Claude maintains a persistent work log across all sessions — daily entries, per-task notes, links, decisions. No manual input needed.

Key commands I use:
- `log this` — captures the current session into the task log
- `prep my standup` — Yesterday / Today / Blockers in under 8 bullets
- `morning brief` — what I left off yesterday, open PRs, top priority
- `task status` — full snapshot of all active work

The log lives locally at `~/work-log/` with an HTML dashboard served via `npx serve`.

---

## Plugins

| Plugin | Role |
|---|---|
| **Figma** (official) | Code Connect, design-to-code, component generation |
| **Pyright LSP** (official) | Inline Python type checking inside Claude Code sessions |
