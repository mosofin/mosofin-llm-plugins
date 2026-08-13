---
name: financial-reports
description: Run MosoFin period financial reports — P&L, balance sheet, cash flow, trial balance, general ledger — for a confirmed workspace and company. Use when the user asks for financial statements, a monthly/quarterly report, margins, or "how did we do" over a period.
allowed-tools:
  - mcp__plugin_mosofin_mosofin__list_workspaces
  - mcp__plugin_mosofin_mosofin__get_agent_datasources
  - mcp__plugin_mosofin_mosofin__get_datasource_tools
  - mcp__plugin_mosofin_mosofin__invoke_datasource_api_tool
---

# Financial reports

Focused report path over the same contract as `/mosofin:query-workspace`.
All rules there apply: workspace confirmation first, companies by
`display_name`, opaque handles on every call, grounding with a Data sources
line. Full spec: `docs/mcp-tool-spec.md`.

## Steps

1. Confirm workspace via `list_workspaces` if this chat has not already
   (single workspace auto-confirms server-side — still read the name back and
   get a yes). Reuse an already-confirmed `ws_…` handle otherwise.
2. `get_agent_datasources` — pick the company by `display_name` if several are
   connected; carry its `data_source_id` on every invoke.
3. Resolve the period to concrete `YYYY-MM-DD` dates before invoking. Never
   silently assume a period — confirm ambiguous phrases ("this quarter" in the
   first week of a quarter usually means the prior one).
4. Invoke the reports the question needs, **in parallel in one turn**:
   - Period (`start_date` + `end_date`): `get_profit_and_loss`,
     `get_cash_flow`, `get_trial_balance`, `get_general_ledger`,
     `get_customer_sales`, `get_vendor_expenses`
   - As-of (optional `report_date`, default today): `get_balance_sheet`
   - Pass `accounting_method` ("Accrual" / "Cash") in params when the user
     specifies one.
5. Present figures from the results only. Check `mock` — say whether numbers
   are live or fixture. End with one **Data sources** line per fetch
   (datasource + tool + `fetched_at`).

For comparisons across companies, invoke once per company and label each
result. For period-over-period, invoke once per period.
