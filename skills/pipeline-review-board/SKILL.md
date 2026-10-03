---
name: pipeline-review-board
description: Build or refresh a weekly pipeline review board in Taskpend — stuck deals, slipped dates, and per-deal next steps — from the user's deal list, CRM export, or description. Use when the user mentions a pipeline review, deal review, stuck deals, or preparing their weekly sales meeting.
---

# Weekly pipeline review board

Turn the user's pipeline (pasted list, CRM export, or described deals) into a review-ready board, so the weekly meeting starts from data instead of memory.

## Structure this skill creates
One container ("Pipeline Review" or the user's name for it) with:
- One task per deal; deal context in the description.
- Columns on the container: **Stage** (status), **Next step** (text), **Close date** (date), **Owner** (text or person), **Risk** (status: On track / At risk / Stuck).
- Group tasks by Stage so the board reads as a kanban.

## Workflow
1. `search_tasks` for an existing pipeline/review container; create one only if missing. **Refresh, don't duplicate**: before creating a deal task, `list_tasks` with `q` = deal name under the container — if it exists, `update_task` / `set_column_value` instead.
2. `list_columns` with `include_inherited: true`; create only the missing columns from the set above.
3. One task per deal via `create_task`; fill column values the user actually stated via `set_column_value` — never invent stage, dates, or amounts.
4. Mark risk only from stated signals (e.g. "no touch in 3 weeks", "slipped twice") and say which signal you used.
5. Finish with a short agenda in the container's document (`get_document` first, then `write_document` preserving existing content): stuck deals on top, then slipped dates, then commits — each line linking the deal by name.

## Rules
- Additive and update-only; never delete deals. Never fabricate pipeline data — a column the user gave no value for stays empty.
- If the user wants a recurring refresh, suggest scheduling this as a routine in Taskpend rather than re-pasting weekly.
