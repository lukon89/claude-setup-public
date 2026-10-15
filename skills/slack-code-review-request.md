# /slack-code-review-request

Draft a Slack code review request for the current branch's open PR.

## Steps

1. Find the open PR for the current branch:
   ```bash
   gh pr view --json url,title,body,headRefName,number
   ```

2. Extract the ticket ID from the branch name or PR body.

3. Ask the user which Slack channel and reviewer to tag if not specified.
   Use `slack_search_channels` / `slack_search_users` MCP tools to find them by name.

4. Draft the message using `slack_send_message_draft` (never send directly) in this format:

```
:eyes: <@REVIEWER_ID> Please review <one-line description> (<TICKET_ID>):
• <PR URL>
```

Rules for the format:
- Open with a review emoji (`:eyes:`, `:opened:`, or similar)
- Tag the reviewer with `<@USER_ID>`
- One sentence: what the PR fixes/adds — plain English, no markdown bold
- Ticket ID or linked inline
- PR URL on its own line, prefixed with `•` (bullet character, not markdown `-`)

5. Show the draft to the user and wait for approval before sending.

## Notes

- Always use `slack_send_message_draft` — never `slack_send_message` directly.
- Pull the description from the PR body's `## Summary` section — keep it to one plain sentence.
- Do not use markdown bold (`**`), headers, or bullet lists in the message body — Slack renders differently from markdown.
