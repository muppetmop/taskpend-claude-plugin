---
name: claude-session-tracker
description: Track every Claude conversation as a Taskpend task, so the user can follow all their Claude work in one place and pick any of it back up, from Taskpend or from a new chat. Use at the start of any conversation where the user asks for real work (a task, research, writing, code, a plan), even if they don't mention Taskpend; keep the task updated as the work moves, and close it out at the end. Also use when the user asks to continue, resume or pick up a Taskpend task or an earlier Claude session ("continue my Taskpend task about X", "where did we leave the pricing page?").
---

# Claude session tracker

Claude conversations are hard to keep track of: dozens of chats, each holding half-finished work, and no single place to see what's running, what's done, and what's waiting on you. This skill gives every conversation a task in Taskpend: a live record of what was asked, where it stands, and how to pick it up again.

## Structure
- One container, **"Claude sessions"**, at the top level of the user's Taskpend, with a **Status** column: `column_type: "status"`, `column_config: {"options": [{"label": "In progress", "color": "#6D28D9"}, {"label": "Waiting on you", "color": "#F59E0B"}, {"label": "Done", "color": "#10B981"}]}`. A status is set with `set_column_value` and the option's label as `value_text`.
- One task per conversation, inside it. Title: what the user wants, in their words, short enough to scan on a phone ("Draft the Q4 pricing page", not "Session 14").
- The task's document is the record. Keep it in this shape:

```markdown
## Goal
What the user asked for, in one or two lines.

## Where it stands
- Done: …
- Next: …
- Waiting on you: … (only when something is)

## Decisions
- … (what was decided, and why, in a line each)

## Results
Links, files, final text, or a short summary of the output.

## Pick up from here
A short brief someone (or another chat) can act on cold: the goal, the current state, the next step, and anything they must not redo.
```

## Workflow
1. **Start.** When the conversation turns into real work, find or create the "Claude sessions" container (`search_tasks` for "Claude sessions"; create it once with `create_task` if missing). Check its columns with `list_columns` (`include_inherited: true`) and create the Status column only if it's missing.
2. **Open the task.** `create_task` with `parent_id` set to the container and a short title, then `write_document` with the Goal and an empty Where it stands, and set Status to *In progress* with `set_column_value`. Tell the user once, in one line, with the link: `https://www.taskpend.com/task/<task id>`.
3. **Keep it current.** When the work reaches a real milestone (a draft is done, a decision is made, the plan changes, you're blocked on the user), rewrite the document with `write_document`. It overwrites, so always send the whole document. Don't update after every message: once per milestone is enough.
4. **Waiting on the user.** When you stop to ask them something, set Status to *Waiting on you* and put the question under "Waiting on you".
5. **Close out.** When the work is done, or the conversation is clearly ending, write the final document (Results filled in, Pick up from here current), set Status to *Done* if the goal was met (and `update_task` with `completed: true`), or leave it *In progress* with the next step if not.

## Picking work back up
- **From a new chat:** when the user asks to continue something, `search_tasks` in their words, show the 1–3 best matches if it's ambiguous, then `get_document` and resume from "Pick up from here". Keep tracking in the **same** task, not a new one.
- **From Taskpend:** the "Pick up from here" brief is written so Taskpend's own agent, or a teammate, can open the task and carry on without this conversation. Keep it self-contained: no "as discussed above".

## Rules
- One task per conversation. If the user switches to an unrelated piece of work, open a second task and say so in a line.
- Skip small talk and one-off questions ("what's 15% of 80?"): only real work gets a task.
- If the Taskpend tools aren't connected or a call fails, carry on with the user's work and say in one line that tracking is off. The tracking never blocks the work.
- If the user says they don't want this conversation tracked, stop, and don't create the task.
- Never put secrets (passwords, API keys, tokens) in the document.
- Write it the way the user talks: their language and their names for things.
