# Repo: muppetmop/taskpend-claude-plugin

## What it is
- Claude Code plugin ("Taskpend for Claude", v0.1.0, MIT). No app code, no build, no package manifest.
- Only 12 files; no WhatsApp code (search "whatsapp" = 0 hits at 5e75cdb).
- All commits since 2026-10-01 are plugin scaffolding (plugin.json, marketplace.json, .mcp.json, README, 1 command, 5 skills).

## Layout
- `.claude-plugin/plugin.json`, `marketplace.json`, `icon.png` — plugin manifest
- `.mcp.json` — remote HTTP MCP server for Taskpend (Supabase edge function `/functions/v1/mcp`); nothing runs locally
- `commands/capture.md` — `/taskpend:capture` slash command
- `skills/<name>/SKILL.md` — capture-to-board, metrics-dashboard, daily-brief-to-board, follow-up-tracker, call-prep-sheet, pipeline-review-board

## Conventions
- Skills: YAML frontmatter (`name`, `description` with "Use when..." trigger) then `# Title`, `## Workflow` (numbered, bold lead-ins), `## Rules`.
- Rules style: additive only, search before create (no duplicates), no invented items, end with a one-line summary.
- Tools referenced by MCP name: search_tasks, create_task, list_tasks, update_task, list_columns, set_column_value.
- README: setup, privacy policy (taskpend.com/privacy), support@taskpend.com.

## Gap
- The WhatsApp remote-control code lives in a different repo (the Taskpend app), not this one.
