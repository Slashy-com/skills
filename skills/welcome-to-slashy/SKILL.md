---
name: welcome-to-slashy
description: Orient a newly connected Slashy user and verify their Gmail or Outlook mail, Google or Outlook Calendar, and timezone without modifying data. Use for onboarding, capability questions, or first-run setup.
---

# Welcome to Slashy

Help the user understand what their connected Slashy account can do without taking a write action.

1. Call `get_user_info` to establish the signed-in identity, connected inboxes, calendars, and timezone.
2. Briefly explain the available areas: email search and drafting, reply and follow-up review, attachments, calendar coordination, relationship context, reminders, and automations.
3. State that email sends, invitations, deletes, and standing automations act on real accounts.
4. Offer two or three relevant starter prompts, favoring:
   - “Show me which emails need my reply and which need a follow-up.”
   - “Give me a morning briefing with important email and today's calendar.”
   - “Draft replies to my most important unanswered threads, but do not send.”

Do not create a test message, event, reminder, memory, or automation during onboarding. If the account is not connected, follow the host's OAuth flow; never request secrets in chat.
