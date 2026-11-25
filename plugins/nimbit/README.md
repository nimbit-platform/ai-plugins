# Nimbit plugin (`nimbit`)

Operate your Nimbit IoT workspace from Claude, Codex, and other AI assistants. This plugin connects your assistant to the Nimbit MCP server and adds six shared skills and three slash commands for everyday plant and operations workflows.

It contains no credentials and no workspace-specific settings. You sign in with your own Nimbit account, and your assistant only ever sees what that account may see.

## What you get

| Skill                   | Use it to                                                              |
| ----------------------- | ---------------------------------------------------------------------- |
| `plant-health-summary`  | Get a one-page health picture of the workspace or a part of it         |
| `investigate-incident`  | Work an incident from symptom to cause, then acknowledge or resolve it |
| `shift-handover`        | Write the handover note for the next shift                             |
| `energy-and-efficiency` | Compare consumption and efficiency across assets and periods           |
| `onboard-a-device`      | Check a new device's type and data, and test its ingest path           |
| `write-a-rule`          | Draft, validate and replay an automation rule (saved as a draft only)  |

| Command     | Runs                   |
| ----------- | ---------------------- |
| `/health`   | `plant-health-summary` |
| `/incident` | `investigate-incident` |
| `/handover` | `shift-handover`       |

Claude Code also accepts the namespaced form, for example `/nimbit:health`.

Skills that change something (acknowledge, assign or resolve an incident, send a report, create a dashboard, start a simulation, save a rule draft) ask you to confirm first. `save_rule_draft` only saves a draft: nothing is enabled or published until you do that in Nimbit.

## Install

### Claude Code

Install via the public marketplace:

```bash
claude plugin marketplace add nimbit-platform/ai-plugins
claude plugin install nimbit@nimbit
```

Or test from a local clone:

```bash
claude --plugin-dir /path/to/ai-plugins/plugins/nimbit
```

Then run `/mcp`, select **nimbit** and sign in. Your browser opens the Nimbit consent screen, where you choose which toolsets Claude may use.

Or add the MCP server on its own:

```bash
claude mcp add --transport http nimbit https://mcp.nimbit.ai/mcp
```

### Claude Desktop and Claude on the web

Open **Settings → Connectors → Add custom connector**, enter `https://mcp.nimbit.ai/mcp` and sign in. The skills and commands in this plugin are for CLI/IDE agents; the connector alone gives Claude the Nimbit tools.

### OpenAI Codex & other Agent Skills clients

- **Codex MCP:** `codex mcp add nimbit --url https://mcp.nimbit.ai/mcp` (or load via `.agents/plugins/marketplace.json` / `.codex-plugin/plugin.json`).
- **Skills (`SKILL.md`):** copy or link subdirectories of `./skills/` into `~/.codex/skills/` (or your client's skills folder).

### Your own Nimbit environment

If your organization runs Nimbit at its own address, use `<your-nimbit-origin>/mcp` instead of `https://mcp.nimbit.ai/mcp`. In the plugin, edit the `url` in `.mcp.json`.

## Toolsets and permissions

Your assistant gets only the toolsets you grant on the consent screen, and only the tools your Nimbit role allows. A workspace admin can restrict which toolsets anyone may grant under **Access → API keys → Connected apps**. If a skill says a tool is not available, the toolset was not granted: reconnect and select it.

The skills read data with the read toolsets (`core`, `assets`, `telemetry`, `events`, `knowledge`, `dashboards`, `canvases`, `actions`). Writing steps need more: `mcp:operations:write` for incidents, `mcp:reports:write` for reports, `mcp:dashboards:write` for dashboards, `mcp:canvases:write` for canvases, `mcp:automation:write` for rule drafts, `mcp:actions:execute` for action requests and triggers, and `mcp:integration` for the device tools.

## Headless use

For automation without a browser, create an API key (`nbk_…`) with `mcp:*` scopes under **Access → API keys** and send it as a bearer token to the same `/mcp` URL.

## License

MIT. See [LICENSE](LICENSE).
