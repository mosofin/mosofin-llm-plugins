---
name: aging-review
description: Review A/R and A/P aging in MosoFin — aged receivables, aged payables, customer and vendor balances. Use when the user asks who owes us, what we owe, overdue invoices or bills, collections, or an aging report.
allowed-tools:
  - mcp__plugin_mosofin_mosofin__list_workspaces
  - mcp__plugin_mosofin_mosofin__get_agent_datasources
  - mcp__plugin_mosofin_mosofin__get_datasource_tools
  - mcp__plugin_mosofin_mosofin__invoke_datasource_api_tool
---

# A/R and A/P aging review

Focused aging path over the same contract as `/mosofin:query-workspace`.
All rules there apply. Full spec: `docs/mcp-tool-spec.md`.

## Steps

1. Confirm workspace (`list_workspaces`) and company (`get_agent_datasources`
   by `display_name`) if not already confirmed in this chat.
2. Invoke, in parallel as needed (all as-of tools; optional `report_date`,
   default today):
   - Receivables: `get_aged_receivables`, `get_customer_balance`
   - Payables: `get_aged_payables`, `get_vendor_balance`
3. To drill into specific overdue items, follow with `search_invoices` or
   `search_bills` scoped by `start_date` / `end_date`, and page through
   `pagination.next_offset` while `has_more` is true.
4. Report by aging bucket; name customers/vendors as returned. Check `mock`
   (live vs fixture) and end with a **Data sources** line per fetch.

MosoFin is read-only: it cannot send reminders, record payments, or write off
balances. Offer the drill-down reads instead if asked.
