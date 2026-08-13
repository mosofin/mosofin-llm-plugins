---
name: workspaces
description: List, confirm, or switch the MosoFin workspace for this conversation (maps to the list_workspaces MCP tool). Use when the user asks which workspaces they have, wants to work in a specific workspace, or wants to switch or add workspaces mid-chat.
allowed-tools:
  - mcp__plugin_mosofin_mosofin__list_workspaces
---

# Workspaces — discover and confirm

Wraps the server's `list_workspaces` tool: the mandatory first gate before any
MosoFin data or skill tool. Full spec: `docs/mcp-tool-spec.md` §3.1.

## Steps

1. Call `list_workspaces` with no arguments (discovery).
2. Outcomes:
   - **0 workspaces** — relay the server message; stop.
   - **1 workspace** — auto-confirms server-side. Read the name back and wait
     for an explicit yes before any data tool runs.
   - **2+ workspaces** (`selection_required`) — ask whether the task is
     single- or multi-workspace, then which workspace(s) **by name**. Never
     auto-pick. Confirm with
     `list_workspaces(workspace_ids=[…], mode="single"|"multi")`.
3. To switch or add a workspace later, re-run the same confirm call with the
   new selection.

## Rules

- Refer to workspaces by **name** (and role) only. Never show integer tenant
  ids; show a `ws_…` handle only if the user asks for a machine reference.
- The server is stateless: every later tool call must carry the confirmed
  `ws_…` handle as `workspace_id`. In multi mode, each call takes **one**
  handle — pick the relevant one per call.
- After confirmation, continue with `/mosofin:connections`,
  `/mosofin:list-tools`, or `/mosofin:query-workspace`.
