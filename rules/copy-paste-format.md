# Copy-Paste Format

When the user asks for something in "copy-paste form", wrap the entire output in a triple-backtick code block with no language tag. This prevents the UI from rendering markdown inside it, so the user sees and copies raw text.

Inside the block: no bold (`**text**`), no headers (`#`), no inline backticks. Plain hyphen bullets (`- item`) and numbered lists (`1. item`) are fine.

**Why:** Even plain-looking text gets rendered as markdown by the Claude UI. Wrapping in a code block is the only way to guarantee the user copies raw text that pastes cleanly into Slack or other tools.

**How to apply:** Any time the user says "copy-paste form", "copy-paste ready", "paste into Slack", or similar — wrap the full output in a plain triple-backtick block with no syntax tag.
