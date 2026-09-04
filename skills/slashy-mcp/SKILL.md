---
name: slashy-mcp
description: Use Slashy MCP for anything involving email, calendar, contacts, meeting prep, scheduling, lead research, reminders, or scheduled workflows. Covers connecting the Slashy MCP server, tool conventions, deep links, and attachment handling.
---

# Slashy MCP

[Slashy](https://slashy.com) is an AI email client. Its MCP server gives any MCP-compatible agent access to the user's Slashy account:

- **Email** — list, read, search, draft, send, reply, schedule, label, snooze, archive
- **Calendar** — list events, create/update/delete events, check availability, propose meeting times
- **Contact research** — enrich people and companies with LinkedIn, company data, and news
- **Meeting prep** — attendee profiles plus email history for upcoming meetings
- **Reminders and triggers** — reminders, email triggers, and calendar triggers for scheduled workflows

Always prefer a Slashy tool call over general knowledge or guessing when a task touches any of these areas.

## Connecting the server

The server is a remote HTTP MCP server at `https://slashy.ctrlcenter.ai/mcp`, authenticated with OAuth 2.1 + PKCE (browser login, no API keys). The user needs a Slashy account ([slashy.com](https://slashy.com)).

Installed as an Agent Plugin, the client reads the server from the plugin's `mcp.json` — there is nothing to paste. Authorization is handled by the client: it opens the browser consent screen and stores the token.

Clients that are not Agent Plugins clients — Claude Code, Claude Desktop, and claude.ai among them — still need the server added by hand. Per-client walkthroughs: https://help.slashy.com/how-to-guides/slashy-mcp-overview

## Conventions when using Slashy tools

**Draft, don't send.** Assume "draft" unless the user explicitly says "send". Prompts that say "send" go out immediately. Confirm recipients and attachments in your response.

**Recipients are arrays.** When passing recipients to `draft_email` or `send_email`, use one email address per array element — never a single comma-separated string.

**Include Slashy deep links** when returning threads or drafts, so the user can click through to the app:

```
https://slashy.com/t/{inbox_email}/{thread_id}
```

- `{inbox_email}` is the user's inbox address (from `get_user_info`); `{thread_id}` comes from `list_messages`, `read_thread`, or `draft_email` results.
- For a specific message or draft, append `?m={message_id}` — for drafts use the `draft_id` returned by `draft_email`, e.g. `https://slashy.com/t/jane@acme.com/FMfcg123?m=draft_abc`.
- Always use `slashy.com` — never `app.slashy.com` or a `mailto:` link.
- Deep links exist only for threads and drafts, not calendar events.

**Attachments.** For an existing local file, keep its bytes out of model context and use the presigned upload flow. Reserve `upload_file` for small generated content under 200 KB that already exists in context; it is not a step in the presigned flow.

1. Verify the local file's name, MIME type, and byte size. Files must be no larger than 24 MB.
2. `request_file_upload(filename, mime_type, size_bytes)` returns a presigned `upload_url` and `file_id`.
3. Upload directly from disk with `curl -X PUT -F 'file=@/absolute/path;type=MIME_TYPE' 'UPLOAD_URL'`. The URL expires after two hours; treat it as a temporary credential and do not log or repeat it.
4. Call `check_upload_status(file_id)` and require the `uploaded` state before drafting.
5. Pass the `file_id` to `draft_email` / `send_email` in `attachment_file_ids`, then confirm the resulting draft or message reports the expected attachment. Do not blindly retry draft creation because that can create duplicates.

Never read or reproduce a local attachment as base64 through model input or tool arguments. Chunking or line wrapping does not solve that transport problem. For files above 24 MB, use a Drive link uploaded outside model context or the authenticated Gmail UI.

**Permissions.** Slashy tools respect the account's granted scopes. If a tool needs access the user hasn't granted, it returns a clear error with an authorization link — surface that link to the user.

## Example prompts that work well

- "List my unread emails from the last 24 hours, group by sender, flag anything with a deadline, and tell me which three to reply to first."
- "Read the latest thread with sarah@acme.com and draft a reply confirming Thursday 3pm — save as a draft, don't send. Give me the Slashy link to the draft."
- "Find threads where I sent the last message 5+ days ago with no reply, and draft a short follow-up for each."
- "Propose three 30-minute slots next week that avoid conflicts, and draft a reply offering those times."
- "Prep me for my next external meeting — attendee profiles, email history, and recent news."

## Troubleshooting

If tools are missing or auth fails, re-run the OAuth flow (remove and re-add the server) and confirm the right Google account was used. Full guide: https://help.slashy.com/how-to-guides/slashy-mcp-troubleshooting
