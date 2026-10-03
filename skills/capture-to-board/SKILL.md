---
name: capture-to-board
description: Turn meeting notes, a brain dump, a plan or any pasted text into a structured Taskpend board — tasks under a container, with a status column. Use when the user wants to capture notes into Taskpend, build a board or task list from a conversation, or says "put this in Taskpend".
---

# Capture to a Taskpend board

Turn unstructured text (meeting notes, a plan, a voice-note transcript, this conversation) into a working board in the user's Taskpend account.

## Workflow

1. **Find or create the container.** Ask where it should live only if the user didn't say. Use `search_tasks` to find an existing container by name; otherwise `create_task` with the board's title (no `parent_id` puts it at the top level of Home).
2. **Extract the tasks.** Each actionable item becomes one task: short imperative title; details go in `description`. Don't invent items that aren't in the source.
3. **Create the structure.** `create_task` each item with `parent_id` = the container. For grouped work (e.g. workstreams), create the group tasks first, then nest items under them.
4. **Add a status column** so the container works as a kanban: `create_column` on the container with `column_type: "status"`, name "Status". Check `list_columns` with `include_inherited: true` first — if a status column already exists in the ancestor chain, reuse it instead of creating a duplicate.
5. **Set initial values** only when the source states them (e.g. "done", "waiting on X") via `set_column_value`. Leave the rest unset.
6. **Report back** with the container's name and what was created, and let the user know they can open it in Taskpend and switch views (outliner / kanban / spreadsheet).

## Rules

- Never delete or overwrite existing tasks or documents while capturing; this skill only adds.
- Respect the user's wording for titles — tidy, don't rewrite meaning.
- If Taskpend tools are unavailable, say the Taskpend connector needs to be connected (Settings → Connectors) rather than simulating output.
