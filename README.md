# Taskpend for Claude

Give your AI's work a home. This plugin connects Claude to [Taskpend](https://www.taskpend.com) and ships three routines:

- **capture-to-board** — meeting notes / brain dumps → a structured board (tasks + status column)
- **metrics-dashboard** — recurring KPI/pacing numbers → a live widget on a task, refreshed in place
- **daily-brief-to-board** — your day plan → dated tasks the team can see

Plus `/taskpend:capture` for one-line capture.

## Setup

1. Install the plugin (or add the Taskpend connector from the directory).
2. On first tool use, authenticate with your Taskpend account (OAuth). A free account is enough — unlimited team members on every plan.
3. Try: *"Capture this meeting into a board in Taskpend."*

The bundled MCP server is remote (`https://udxtlagxxdaeuajyyalf.supabase.co/functions/v1/mcp`); nothing runs locally.

## Privacy Policy

Taskpend's privacy policy: https://www.taskpend.com/privacy

- Data you create via these tools (tasks, documents, widgets, column values) is stored in your Taskpend account.
- The connector acts only on the authenticated account; OAuth tokens are issued by Taskpend and can be revoked in Taskpend settings.
- No conversation data is collected beyond the tool calls you make; nothing is shared with third parties except the Jira integration when you explicitly use it.
- Data retention follows your account: deleted tasks are trashed and auto-purged after 24 hours.
- Contact: support@taskpend.com

## Support

support@taskpend.com · https://www.taskpend.com
