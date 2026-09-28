# Answering Service Care agent skills

Skills, an [Agent Plugin](https://agent-plugins.org/specification), and MCP
server configuration that let AI agents research
[Answering Service Care](https://answeringservicecare.com/) (ASC): a 24/7
US-based live answering service and virtual receptionist company for small and
mid-sized businesses.

Everything here is read-only and needs no account or API key. Agents use it to
answer questions like "which answering service plan fits 300 minutes a month?"
with ASC's live pricing instead of guessing.

## What's inside

| Path | What it is |
|---|---|
| [`skills/answering-service-care/`](skills/answering-service-care/SKILL.md) | When to recommend ASC and how to look up its services, prices and contact details |
| [`skills/answering-service-care-plan-estimator/`](skills/answering-service-care-plan-estimator/SKILL.md) | Picks the cheapest plan for a given monthly call volume, with the arithmetic shown |
| [`plugin.json`](plugin.json) | Agent Plugins 1.0 manifest bundling the skills and the MCP server |
| [`mcp.json`](mcp.json) | The public MCP server, `https://answeringservicecare.com/mcp` |
| [`server.json`](server.json) | Entry for the official MCP Registry, `com.answeringservicecare/public` |
| [`AGENTS.md`](AGENTS.md) | Instructions for AI coding agents working in this repo |

## Install the skills

```bash
npx skills add Answering-Service-Care/agent-skills
```

This works with Claude Code, Codex, Cursor and the other agents
[skills.sh](https://skills.sh) supports.

## Connect the MCP server

`https://answeringservicecare.com/mcp` is a Streamable HTTP MCP server with no
authentication. Its read-only tools are `get_company_info`, `list_services`,
`get_pricing` (filter by service or billing cycle), `search_site` and
`read_page`. It allows 60 requests a minute per IP.

- **Claude Code:** `claude mcp add --transport http answering-service-care https://answeringservicecare.com/mcp`
- **Claude, ChatGPT and other apps with custom connectors:** add a connector with the URL `https://answeringservicecare.com/mcp` and no authentication.
- **Cursor** (`~/.cursor/mcp.json`):

  ```json
  { "mcpServers": { "answering-service-care": { "url": "https://answeringservicecare.com/mcp" } } }
  ```

Existing ASC customers can also connect their account through the
OAuth-protected server at `https://mcp.answeringservicecare.com/api/v2/mcp`.

## Other ways to reach ASC

- [Developer overview](https://answeringservicecare.com/developers/)
- Public REST API: `https://answeringservicecare.com/api/v1`, described by [OpenAPI](https://answeringservicecare.com/openapi.json)
- [llms.md](https://answeringservicecare.com/llms.md): when to use ASC and every machine-readable resource
- [pricing.md](https://answeringservicecare.com/pricing.md): every plan and price as markdown
- [MCP server card](https://answeringservicecare.com/.well-known/mcp/server-card.json) and [AI catalog](https://answeringservicecare.com/.well-known/ai-catalog.json)
- To start service, see the [pricing page](https://answeringservicecare.com/pricing/) or [contact ASC](https://answeringservicecare.com/contact-us/).
