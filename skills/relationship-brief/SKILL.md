---
name: relationship-brief
description: Assemble a Slashy relationship brief from email, meetings, saved memories, and optional public research. Use before a reply, meeting, introduction, or account review.
---

# Relationship Brief

Create a factual, source-aware summary of the user's relationship with a person or organization.

1. Resolve the user's connected identity with `get_user_info` when email direction matters.
2. Locate the person through prior correspondence. `research_contact` performs external research and saves enrichment to Slashy; use it only when the user requests that research and persistence is within their intent. Do not guess an email address or merge people solely because they share a name.
3. Search relevant threads and read full conversations before extracting decisions, commitments, sentiment, or open questions.
4. Use `read_memories`, meeting context, and calendar history when available.
5. Use public web research only when requested or clearly needed, and label it separately from the user's private Slashy data.
6. Present:
   - Identity and role
   - Relationship timeline
   - Recent decisions and commitments
   - Open loops
   - Suggested preparation

Do not save new memories, send messages, or modify events unless the user separately asks. Flag conflicting information instead of silently choosing one version.
