---
name: find-transactions
description: Search MosoFin transactions and records — invoices, bills, payments, purchases, deposits, estimates, credit memos, journal entries, customers, vendors, accounts, items. Use when the user wants to find, list, count, or look up specific transactions or a record by id.
allowed-tools:
  - mcp__plugin_mosofin_mosofin__list_workspaces
  - mcp__plugin_mosofin_mosofin__get_agent_datasources
  - mcp__plugin_mosofin_mosofin__get_datasource_tools
  - mcp__plugin_mosofin_mosofin__invoke_datasource_api_tool
---

# Find transactions and records

Focused search path over the same contract as `/mosofin:query-workspace`.
All rules there apply. Full spec: `docs/mcp-tool-spec.md`.

## Steps

1. Confirm workspace (`list_workspaces`) and company (`get_agent_datasources`
   by `display_name`) if not already confirmed in this chat.
2. Pick the tool from the live catalog (`get_datasource_tools`) — e.g.
   `search_invoices`, `search_bills`, `search_payments`, `search_customers`,
   `search_vendors`, `search_accounts`, `search_items`,
   `search_journal_entries`, `search_purchases`, `search_deposits`,
   `search_estimates`, `search_credit_memos`. Never invent a `tool_name`.
3. Searches require concrete `start_date` and `end_date` (`YYYY-MM-DD`) —
   resolve relative phrases first.
4. **Page to completion** when counting or summing: while
   `pagination.has_more` is true, re-invoke with
   `params.offset = pagination.next_offset`. Partial pages must never be
   presented as totals — say when results are truncated.
5. For one known record, use the get-by-id tools (`get_customer`,
   `read_invoice`, `get_vendor`, `get_account`, …) with `id`.
6. Check `mock` (live vs fixture) and end with a **Data sources** line per
   fetch.
