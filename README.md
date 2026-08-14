# MosoFin

Connect Claude Code or ChatGPT to your QuickBooks and business data through
the MosoFin MCP server. Sign in once; the plugin is read-only and scoped to
the workspace you confirm.

## Install

In Claude Code:

```text
/plugin marketplace add mosofin/mosofin-llm-plugins
/plugin install mosofin@financehub
```

Then:

1. Complete MosoFin sign-in in the browser when prompted.
2. Confirm the workspace for this chat.
3. Ask a business-data question, or run `/mosofin:query-workspace`.

## Update

Claude Code does not pick up GitHub changes automatically. After this repo
updates, refresh the marketplace and the plugin:

```text
/plugin marketplace update financehub
/plugin update mosofin@financehub
```

Codex and ChatGPT talk to the live MosoFin MCP server, so server tools update
without this step. Skill text in this repo updates only after you refresh the
plugin as above.

## Skills

| Skill | When to use |
|-------|-------------|
| `/mosofin:workspaces` | List, confirm, or switch the workspace for this chat |
| `/mosofin:connections` | See which company files are connected |
| `/mosofin:list-tools` | What API operations are available |
| `/mosofin:run-tool` | Run one named catalog operation |
| `/mosofin:list-skills` | List saved MosoFin skills in the workspace |
| `/mosofin:replay-skill` | Replay a saved skill after you confirm |
| `/mosofin:query-workspace` | Any open data question, end to end |
| `/mosofin:save-skill` | Save a proven workflow after results exist |

## License

MIT. See [LICENSE](LICENSE).
