# Nimbit AI Plugins

Official plugins, shared Agent Skills, and MCP server configuration for operating your [Nimbit](https://nimbit.ai) IoT workspace from AI assistants including Claude Code, Claude Desktop / Web, OpenAI Codex, and other MCP or Agent Skills clients.

Everything here is credential-free and workspace-agnostic: you sign in with your own Nimbit account via OAuth (or use an API key for headless automation), and assistants only ever see what your Nimbit role and workspace MCP policy allow.

## Plugins

| Plugin                      | Path                               | Includes                                                                       |
| --------------------------- | ---------------------------------- | ------------------------------------------------------------------------------ |
| [`nimbit`](./plugins/nimbit) | [`plugins/nimbit`](./plugins/nimbit) | Hosted Nimbit MCP connector (`https://mcp.nimbit.ai/mcp`), 6 skills, 3 commands |

See [`plugins/nimbit/README.md`](./plugins/nimbit/README.md) for the full skill, slash-command, and toolset reference.

## Quick start

### Claude Code

Add this marketplace and install the `nimbit` plugin:

```bash
claude plugin marketplace add nimbit-platform/ai-plugins
claude plugin install nimbit@nimbit
```

Or load a local clone directly without installing from the marketplace:

```bash
claude --plugin-dir ./plugins/nimbit
```

Then run `/mcp`, select **nimbit**, and sign in with your Nimbit account. Or add only the MCP server without skills/commands:

```bash
claude mcp add --transport http nimbit https://mcp.nimbit.ai/mcp
```

### Claude Desktop and Claude on the web

Open **Settings → Connectors → Add custom connector**, enter `https://mcp.nimbit.ai/mcp`, and sign in.

### OpenAI Codex

This repository includes a Codex marketplace catalog at [`.agents/plugins/marketplace.json`](./.agents/plugins/marketplace.json) and plugin manifest at [`plugins/nimbit/.codex-plugin/plugin.json`](./plugins/nimbit/.codex-plugin/plugin.json) pointing to the shared [`skills/`](./plugins/nimbit/skills) and [`.mcp.json`](./plugins/nimbit/.mcp.json).

- **Plugin directory / marketplace:** browse or install `nimbit` via `/plugins` from your local checkout or add this repository as a marketplace source.
- **MCP server only:**

  ```bash
  codex mcp add nimbit --url https://mcp.nimbit.ai/mcp
  ```

- **Skills only:** copy or symlink [`plugins/nimbit/skills/*`](./plugins/nimbit/skills) into `~/.codex/skills/` (personal) or `.codex/skills/` (project).

### Other MCP & Agent Skills clients (Cursor, Gemini CLI, etc.)

Configure your client's HTTP MCP server to `https://mcp.nimbit.ai/mcp` (OAuth 2.1 / PKCE supported) and load the portable `SKILL.md` skills from [`plugins/nimbit/skills/`](./plugins/nimbit/skills).

### Self-hosted / custom Nimbit environments

If your organization runs Nimbit at its own origin, replace `https://mcp.nimbit.ai/mcp` with `<your-nimbit-origin>/mcp` (or edit `url` in [`plugins/nimbit/.mcp.json`](./plugins/nimbit/.mcp.json)).

### Headless / CI automation

Create an API key (`nbk_…`) with `mcp:*` scopes in Nimbit under **Access → API keys** and send it as `Authorization: Bearer <token>` to `/mcp`.

## Repository layout

```text
.
├── .agents/plugins/marketplace.json         # OpenAI Codex plugin marketplace catalog
├── .claude-plugin/marketplace.json          # Claude Code plugin marketplace catalog
├── LICENSE                                  # MIT License
├── README.md
└── plugins/
    └── nimbit/
        ├── .claude-plugin/plugin.json       # Claude Code plugin manifest
        ├── .codex-plugin/plugin.json        # OpenAI Codex plugin manifest
        ├── .mcp.json                        # Shared HTTP MCP server config
        ├── LICENSE
        ├── README.md                        # Plugin skills, commands, and toolsets guide
        ├── commands/                        # Slash commands (/health, /incident, /handover)
        └── skills/                          # Shared portable SKILL.md workflows
```

## License

MIT. See [LICENSE](./LICENSE).
