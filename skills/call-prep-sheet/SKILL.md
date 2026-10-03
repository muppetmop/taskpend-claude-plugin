---
name: call-prep-sheet
description: Turn tomorrow's call list (or this week's meetings) into ordered call-prep tasks in Taskpend, each with a one-page prep doc built from what the user pastes — notes, last emails, CRM context. Use when the user preps calls or demos, mentions "tomorrow's calls", or wants prep sheets.
---

# Call-prep sheet

End-of-day prep is what separates the quota-hitters. This skill turns a call list into an ordered board with a prep page per call.

## Structure
One container ("Call prep — <date>") with one task per call, ordered by time, columns: **Time** (date/text as given), **Account** (text), **Goal** (text).

## Workflow
1. Create the day's container under the user's prep/sales folder (`search_tasks` first; reuse an existing same-date container instead of duplicating).
2. One task per call: title "<time> — <account/person>". Set the columns from what the user provided.
3. For each call, write the prep page with `write_document` on that task (its own doc, so `get_document` only if the task existed before): **Who** / **Last touch** / **Goal of this call** / **Likely objections** / **The one question to ask** — built ONLY from the material the user pasted. Unknown sections get "—", never invented facts.
4. Close with the ordered list for the morning: times + accounts + the one-line goal each.

## Rules
- No invented history or objections — a prep sheet with a wrong "fact" is worse than an empty one.
- If the user pastes nothing about an account, the prep page holds the goal they stated and placeholders only.
