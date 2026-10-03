---
name: follow-up-tracker
description: Capture follow-ups ("waiting on X", "ping Y next week") into a Taskpend follow-up board with next-touch dates, and list what's due today. Use when the user mentions follow-ups they owe, people they're waiting on, or asks what they should chase today.
---

# Follow-up tracker

Most deals die in the follow-up. This skill keeps every "waiting on" in one board with a next-touch date.

## Structure
One container ("Follow-ups") with columns: **Waiting on** (text — who), **Next touch** (date), **Channel** (text), **Status** (status: Waiting / Nudged / Replied / Closed).

## Workflow
1. `search_tasks` for the follow-ups container; create once if missing, with the columns above (`list_columns` first, create only what's missing).
2. Each follow-up = one task: title "Follow up: <person/company> — <topic>". Dedupe by title with `list_tasks q:` before creating; an existing one gets its **Next touch** updated instead.
3. Dates: only what the user stated ("next week" → the coming Monday unless they said otherwise — say which date you chose).
4. "What's due?" → `list_tasks` on the container, read **Next touch** values via `list_columns` + the tasks' column values, and answer with today's and overdue items, oldest first.
5. When the user reports a reply, set Status accordingly and ask if there's a new next touch — don't close silently.

## Rules
- Never invent commitments or dates. Additive and update-only.
- Keep titles short enough to scan on a phone.
