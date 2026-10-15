# /analyse-pr-comments

Fetch all review comments and reviews on the current PR, then for every comment NOT written by the current user: assess, fix if needed, and always reply.

**Comments written by the current user (PR author) must be completely ignored — no reply, no fix.**

## Workflow per comment

1. **Assess** — decide whether the comment requires a code change, a clarification, or can be dismissed:
   - Genuine bug / correctness issue? → Fix it.
   - Style / naming / doc suggestion? → Fix it if it improves clarity.
   - Factually wrong or already handled? → Reply explaining why, no fix needed.
   - Question or nitpick? → Reply with reasoning, fix only if it helps.

2. **Fix** (if needed) — make the code change, run tests locally to confirm nothing broke, then commit and push. **You must still reply after fixing — a fix without a reply is incomplete.**

3. **Reply** — MANDATORY for every comment, no exceptions. Post the reply to the individual comment thread (never just a top-level review comment) **before moving on to the next comment**:
   - Fixed: "Fixed in commit <sha>: <one sentence on what changed and why>."
   - Not fixed: "Not changed: <one sentence explaining why>."

## How to get the PR number

If a PR URL was provided, extract the number from it.
Otherwise: `gh pr view --json number --jq .number`

## How to fetch comments

Always paginate — the default page size is 30.

```bash
# Inline review comments — fetch ALL pages
gh api "repos/{owner}/{repo}/pulls/{pr}/comments?per_page=100" --paginate \
  --jq '.[] | {id, user: .user.login, body, html_url, in_reply_to_id}'

# Top-level review bodies
gh api repos/{owner}/{repo}/pulls/{pr}/reviews \
  --jq '.[] | {id, user: .user.login, body, state, html_url}'
```

After fetching:
1. Drop all comments written by the current user.
2. Drop reply comments (where `in_reply_to_id` is non-null) — only process top-level threads.
3. For each remaining top-level comment, check whether the current user has already replied. Only skip if a reply already exists.

## How to reply to an inline comment

```bash
gh api repos/{owner}/{repo}/pulls/{pr}/comments/{comment_id}/replies \
  -X POST -f body="<your reply>"
```

## How to reply to a top-level review

```bash
gh api repos/{owner}/{repo}/issues/{pr}/comments \
  -X POST -f body="<your reply>"
```

## After all replies: check the pipeline

Run `gh pr checks <pr>` and report the result. If any check is failing:
1. Fetch the failing job's log: `gh run view <run_id> --job <job_id> --log 2>&1 | grep -E "FAILED|ERROR|error|failed" | head -40`
2. Diagnose the root cause
3. Fix it, commit and push
4. Run `gh pr checks <pr>` again to confirm

Do not report the skill as done until all required checks pass.

## Important rules

- Comments by the current user: **skip entirely**.
- All other comments: **always reply — no exceptions**.
- Reply to the specific comment thread, not as a standalone review.
- Keep replies concise: one sentence on what was done and why.
- After all replies are posted, give the user a summary table: comment | decision | reply posted.
