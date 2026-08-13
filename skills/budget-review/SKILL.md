---
name: budget-review
description: Review budgets in MosoFin and compare them to actuals. Use when the user asks about a budget, budget vs actuals, variance against plan, or which budgets exist.
allowed-tools:
  - mcp__plugin_mosofin_mosofin__list_workspaces
  - mcp__plugin_mosofin_mosofin__get_agent_datasources
  - mcp__plugin_mosofin_mosofin__get_datasource_tools
  - mcp__plugin_mosofin_mosofin__invoke_datasource_api_tool
---

# Budget review

Focused budget path over the same contract as `/mosofin:query-workspace`.
All rules there apply. Full spec: `docs/mcp-tool-spec.md`.

## Steps

1. Confirm workspace (`list_workspaces`) and company (`get_agent_datasources`
   by `display_name`) if not already confirmed in this chat.
2. Budgets are **two-step** — never skip step one:
   1. `search_budgets` (optionally `include_details: false`) to list budget
      names. If several match, ask which by name.
   2. `get_budget_details` with `budget_id` or `budget_name` for the numbers.
3. For budget vs actuals, also invoke `get_profit_and_loss` for the same
   period (`start_date` + `end_date`, `YYYY-MM-DD`) — the two invokes in
   step 3 may run in parallel once the budget is chosen — and compute
   variances from the two results only.
4. Check `mock` (live vs fixture) and end with a **Data sources** line per
   fetch.
