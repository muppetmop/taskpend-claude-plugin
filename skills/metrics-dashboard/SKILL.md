---
name: metrics-dashboard
description: Build or refresh a live metrics dashboard widget on a Taskpend task from numbers the user provides or that were produced earlier in the conversation. Use for recurring reporting routines — pacing dashboards, KPI snapshots, weekly metrics — where the user wants the result to live in Taskpend rather than in the chat.
---

# Metrics dashboard in a Taskpend task

Keep a recurring metrics view (pacing, KPIs, weekly numbers) as a live widget on a Taskpend task, so each refresh updates the same place the team already looks at.

## Workflow

1. **Locate the dashboard task.** `search_tasks` by the name the user used ("pacing", "KPI board"…). If none exists, `create_task` for it and say so.
2. **Check for an existing widget.** `list_widgets` on the task. If a dashboard widget from a previous run exists, UPDATE it (`upsert_widget` with its `widget_id`) instead of adding a second one — refresh, don't accumulate.
3. **Build the widget.** `upsert_widget` with self-contained `html`/`css`/`js`: metric cards with current vs target, simple bars or deltas, and a "Last updated" timestamp. No external scripts or network calls; data is baked in or kept in `state`.
4. **Keep history if asked.** Prior values can live in the widget `state` (e.g. an array of snapshots) so the widget can show trend arrows between refreshes.
5. **Summarize in the document** (optional): if the user wants a written trail, append a dated one-paragraph summary via the task document — read it first with `get_document`, then `write_document` with the existing content plus the new entry. Never replace the document without carrying its current content forward.

## Rules

- Numbers come from the user or this conversation only — never invent or extrapolate values into the dashboard.
- One dashboard = one widget, updated in place.
- `write_document` overwrites: always `get_document` first and preserve existing content.
