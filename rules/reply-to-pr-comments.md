# Reply to PR Comments

After fixing something raised in a PR review comment (inline or general), always post a reply to that specific comment confirming it's been addressed.

**Why:** The reviewer has no visibility that the comment was acted on unless explicitly replied to. Silently pushing a fix leaves the thread open and the reviewer confused.

**How to apply:** Immediately after pushing the fix commit, reply to the comment thread — even a brief "Fixed in <commit sha or description>" is enough. Do this in the same turn as the fix, not as an afterthought.

**Use `gh api` to reply inline in the thread:**
```bash
gh api repos/{owner}/{repo}/pulls/{pull_number}/comments/{comment_id}/replies \
  --method POST \
  -f body="Fixed in <sha> — <one line description>"
```

The `comment_id` is the numeric ID on the review comment (visible in its URL fragment, e.g. `#discussion_r4097313764` → `4097313764`).
