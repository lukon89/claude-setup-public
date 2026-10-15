# Slack — Draft Only

When composing any Slack message on the user's behalf — code review requests, status updates, notifications, anything — always use `slack_send_message_draft` so the user reviews and sends it themselves. Never call `slack_send_message` to post directly to a channel without explicit per-turn approval.

**Why:** Posting to Slack is visible to others and cannot be undone cleanly. The user should always review the message before it goes out.

**How to apply:** Any time the task involves sending a Slack message — regardless of how the request is phrased ("post this", "send a message", "notify the team") — default to drafting. Only post directly if the user says "send it now" or equivalent in the same turn.
