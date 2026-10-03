---
name: daily-brief-to-board
description: Turn a daily brief, standup summary or day plan into dated Taskpend tasks. Use when the user reviews their day/plan with Claude and wants the outcome tracked in Taskpend — today's priorities as tasks with dates, follow-ups captured where the team can see them.
---

# Daily brief into Taskpend

Turn the day's plan — a brief, a standup, "here's what I need to do today" — into tracked tasks.

## Workflow

1. **Find the destination.** A recurring container like "Today" / the user's named planning board: `search_tasks` first, create only if missing (and tell the user).
2. **One task per commitment.** `create_task` for each item with a crisp title; context into `description`. Follow-ups on other people get "Waiting:" prefixed or a dedicated group — ask once for the user's convention if unclear, then stick to it.
3. **Dates.** If the container has a date column (`list_columns` with `include_inherited: true`), set today's/the stated date with `set_column_value` (`value_date`). If none exists, create one date column named "Due" on the container first.
4. **Completions.** Items the user reports as already done: create with `completed: true` only if they explicitly want them logged; otherwise skip.
5. **Close the loop.** End with a one-line summary of what landed where, so the user can open the board.

## Rules

- Additive only — never modify or delete tasks that already exist unless the user asks for a specific update (`update_task`).
- Don't duplicate: before creating, check the destination with `list_tasks` (`q` filter) for an existing task with the same title; update it instead.
- No invented commitments: only items the user actually stated.
