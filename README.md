# MosoFin Claude Code plugin

Connect Claude Code to the remote **MosoFin MCP server** and follow the
workspace-confirm → datasource → invoke workflow. This plugin is a packaging
layer: it does **not** copy or run the FastAPI MCP app.

- Production MCP: `https://mcp.mosofin.com/mcp`
- Auth: OAuth 2.0 (dynamic client registration + PKCE). First use opens a browser.
- Contract: [docs/plugin-contract.md](docs/plugin-contract.md)
- Full MCP tool spec (all 7 tools, examples): [docs/mcp-tool-spec.md](docs/mcp-tool-spec.md)

## Install from GitHub

This repository is also a one-plugin marketplace
(`.claude-plugin/marketplace.json`), so once it is pushed to a **public**
GitHub repository, anyone can install it:

```text
/plugin marketplace add mosofin/mosofin-llm-plugins
/plugin install mosofin@financehub
```

Or from the shell: `claude plugin install mosofin@financehub` after adding the
marketplace with `claude plugin marketplace add mosofin/mosofin-llm-plugins`.

## Install (local development)

```bash
claude --plugin-dir /path/to/mosofin-llm-plugins
```

Then:

1. Complete MosoFin sign-in in the browser when prompted.
2. Run `/mcp` and confirm `plugin:mosofin:mosofin` is connected.
3. Ask a business-data question, or run `/mosofin:query-workspace`.

Reload after edits: `/reload-plugins`.

Validate before sharing:

```bash
claude plugin validate . --strict
```

## Staging / tunnel

On enable, Claude Code prompts for **MosoFin MCP URL** (default production).
Point it at a tunnel or staging host when testing, for example:

```text
https://<your-tunnel>/mcp
```

The server's OAuth metadata must advertise that same URL.

## Skills

One skill per MCP server capability, plus the two end-to-end workflows:

| Skill | Server tool | When to use |
|-------|-------------|-------------|
| `/mosofin:workspaces` | `list_workspaces` | List, confirm, or switch the workspace for this chat |
| `/mosofin:connections` | `get_agent_datasources` | Which company files are connected, status, reconnect guidance |
| `/mosofin:list-tools` | `get_datasource_tools` | What API operations are available and their policy |
| `/mosofin:run-tool` | `invoke_datasource_api_tool` | Run one named catalog operation |
| `/mosofin:list-skills` | `get_skills` | List saved MosoFin skills in the workspace |
| `/mosofin:replay-skill` | `get_my_skill` (+ invoke) | Replay a saved skill after explicit consent |
| `/mosofin:query-workspace` | full read pipeline | Any open data question, end to end |
| `/mosofin:save-skill` | `create_skill` (+ list/replay) | Save a proven workflow after results exist |

All skills share the same gates: workspace confirmation first, companies by
`display_name`, opaque handles only, grounding with provenance.

## Agents

| Agent | When to use |
|-------|-------------|
| `mosofin:mosofin-analyst` | Delegate a data-fetch task after the workspace (and company) are confirmed in chat. Read-only; returns figures with provenance |

## Layout

```text
.claude-plugin/
  plugin.json        # manifest (name, version, userConfig for the MCP URL)
  marketplace.json   # makes this repo installable as a marketplace
.mcp.json            # remote MosoFin MCP server (Streamable HTTP + OAuth)
skills/              # query-workspace, save-skill
agents/              # mosofin-analyst subagent
docs/                # plugin-contract.md, mcp-tool-spec.md
```

Intentionally absent: `commands/` (legacy flat-file skills — superseded by
`skills/`), `hooks/` (no events to intercept), `.lsp.json` (no language
server), `monitors/` (nothing to watch in the background), `bin/` (no bundled
executables), and `settings.json` (it only supports overriding the main-thread
agent, which this plugin should not do).

Marketplace skill *content* still lives on the MosoFin server (`get_skills` /
`get_my_skill`). These plugin skills only teach Claude when and how to call MCP.

## What this plugin does not include

- The MosoFin MCP Python server, Docker, or database
- Secrets, JWT keys, or OAuth client ids
- Copies of marketplace `SKILL.md` bundles
- The `@mosofin/mcp` npx stdio bridge (Claude Desktop / Cursor). Claude Code
  speaks remote HTTP MCP natively.

## License

MIT. See [LICENSE](LICENSE).
