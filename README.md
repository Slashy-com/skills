# Slashy Skills

Agent skills and the Agent Plugin for [Slashy](https://slashy.com), the AI email client.

## Install as an Agent Plugin

The plugin bundles the `slashy-mcp` skill **and** the Slashy MCP server config, so a
compatible client wires up both in one step — no hand-editing MCP config.

**Codex CLI:**

```bash
codex plugin marketplace add Slashy-com/skills
codex plugin add slashy@slashy
```

**Cursor:** install from the Cursor Marketplace, or clone this repo into
`~/.cursor/plugins/local/slashy` and reload the window:

```bash
git clone https://github.com/Slashy-com/skills ~/.cursor/plugins/local/slashy
```

**VS Code / GitHub Copilot:** run **Chat: Install Plugin From Source** and give it
`https://github.com/Slashy-com/skills`.

The first Slashy tool call opens the OAuth consent screen in your browser. You need a
[Slashy account](https://slashy.com).

## Install the skill on its own

For agents that load skills but not plugins:

```bash
npx skills add Slashy-com/skills
```

Works with Claude Code, Cursor, Codex, Copilot, Windsurf, and any other agent supported by
the [skills CLI](https://skills.sh). You will need to add the MCP server yourself — see the
[per-client guides](https://help.slashy.com/how-to-guides/slashy-mcp-overview).

## Contents

| Path | What it is |
|------|------------|
| [`plugin.json`](plugin.json) | Agent Plugin manifest ([spec 1.0.0](https://agent-plugins.org)). This repo root is the plugin. |
| [`mcp.json`](mcp.json) | Declares the Slashy MCP server, so clients configure it on install. |
| [`skills/slashy-mcp`](skills/slashy-mcp/SKILL.md) | Teaches your agent to use the Slashy MCP server for email, calendar, meeting prep, contact research, and scheduled workflows — including deep links and attachment handling. |
| [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json) | Codex marketplace catalog, so `codex plugin marketplace add` can install from this repo. |

## Docs

- [Slashy MCP overview](https://help.slashy.com/how-to-guides/slashy-mcp-overview)
- [Using Slashy MCP](https://help.slashy.com/how-to-guides/slashy-mcp-using)
- [Troubleshooting](https://help.slashy.com/how-to-guides/slashy-mcp-troubleshooting)
