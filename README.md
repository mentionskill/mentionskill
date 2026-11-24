# MentionSkill

Free, relevant links and brand mentions for your site, traded by the AI that writes your content.

MentionSkill is a network of sites that link to each other where it helps the reader. While your agent writes or edits a post, it adds one fitting link to a member's page and earns credits. Those credits pay for members to link to the pages you want to rank and be recommended for. No cash, no outreach.

This repo is the installable part: the **MCP connection** (`https://mentionskill.com/mcp`), which gives your agent the MentionSkill tools, and the **skill** ([`skills/mentionskill/SKILL.md`](skills/mentionskill/SKILL.md)), which teaches it the workflow. You sign in with your email the first time your agent connects; no API key needed.

Full instructions with one-click buttons: **https://mentionskill.com/#install**. For agents: **https://mentionskill.com/llms.txt**.

## Install

| Agent | Install (MCP connection + skill) |
|---|---|
| **Claude Code** | `claude plugin marketplace add mentionskill/mentionskill && claude plugin install mentionskill@mentionskill`, then `/mcp` to sign in |
| **Codex** (CLI, IDE, app) | `codex mcp add mentionskill --url https://mentionskill.com/mcp && codex mcp login mentionskill`, and `npx skills add mentionskill/mentionskill -a codex -g -y` |
| **Claude** (web, desktop) | Settings → Connectors → Add custom connector → `https://mentionskill.com/mcp`. Skill: upload [the zip](https://github.com/mentionskill/mentionskill/releases/latest/download/mentionskill-skill.zip) in Settings → Capabilities → Skills |
| **ChatGPT** | Plugins → + → Add custom MCP server → `https://mentionskill.com/mcp` (Business, Enterprise and Edu plans) |
| **Cursor** | One-click button at [mentionskill.com/#install-cursor](https://mentionskill.com/#install-cursor), then `npx skills add mentionskill/mentionskill -a cursor -g -y` |
| **VS Code + Copilot** | One-click button at [mentionskill.com/#install-vscode](https://mentionskill.com/#install-vscode), or add `mentionskill/mentionskill` to `chat.plugins.marketplaces` |
| **Copilot CLI** | `copilot plugin install mentionskill/mentionskill` |
| **Antigravity** | `agy plugin install https://github.com/mentionskill/mentionskill` |
| **Gemini CLI** | `gemini extensions install https://github.com/mentionskill/mentionskill` |
| **Anything else** | `npx add-mcp https://mentionskill.com/mcp -n mentionskill -g -y` and `npx skills add mentionskill/mentionskill -g -y` |

Clients without OAuth can send an API key instead: connect once from an OAuth client, ask it to run `create_api_key`, and send `Authorization: Bearer msk_…`.

## What's here

| Path | For |
|---|---|
| `skills/mentionskill/` | The Agent Skill ([agentskills.io](https://agentskills.io) format) |
| `plugin.json`, `mcp.json` | [Agent Plugins](https://agent-plugins.org) 1.0: VS Code, Copilot, Codex, Cursor |
| `.claude-plugin/`, `.mcp.json` | Claude Code plugin and marketplace |
| `.agents/plugins/marketplace.json` | Codex marketplace |
| `.cursor-plugin/marketplace.json` | Cursor "Import from GitHub" |
| `gemini-extension.json`, `GEMINI.md` | Gemini CLI extension |
| `mcp_config.json` | Antigravity plugin |
| `server.json` | [MCP Registry](https://registry.modelcontextprotocol.io) entry `com.mentionskill/mentionskill` |

The skill is MIT licensed. Using the MentionSkill network is free; see [mentionskill.com](https://mentionskill.com).
