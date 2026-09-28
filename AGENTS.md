# AGENTS.md

Instructions for AI coding agents working in this repository.

## What this repo is

Public, distributable agent material for Answering Service Care (ASC): two
Agent Skills, an Agent Plugins 1.0 manifest, the MCP client config for ASC's
public MCP server, and that server's MCP Registry entry. It contains no
application code. The MCP server itself runs as a Cloudflare Worker whose
source is private; its tools are listed in the README.

## Layout

- `skills/<name>/SKILL.md`: one Agent Skill per directory. The frontmatter
  `name` must equal the directory name (lowercase letters, digits and hyphens,
  at most 64 characters), and `description` must say when to use the skill in
  at most 1024 characters.
- `plugin.json`: Agent Plugins manifest. Only `$schema` and `name` are
  required; the schema allows no other top-level fields than those in
  https://agent-plugins.org/schemas/1.0.0/plugin.schema.json.
- `mcp.json`: MCP servers for the plugin. Server config must live here, never
  inline in `plugin.json`.
- `server.json`: MCP Registry entry. `description` is limited to 100
  characters, and `name` must stay under the `com.answeringservicecare/`
  namespace, which the registry verifies through a DNS record on
  answeringservicecare.com.

## Rules

- Keep facts out of the skills. Prices, plans and contact details change, so
  skills tell agents to call the API (`/api/v1/*`) or MCP tools rather than
  quoting numbers.
- The site serves the same two skills at
  https://answeringservicecare.com/.well-known/agent-skills/index.json. When you
  change a skill here, the copy in the website's theme needs the same change.
- When the MCP server's version changes, bump `version` in `server.json` and
  `plugin.json` together, then republish with `mcp-publisher publish`.
- Every URL must point at `https://answeringservicecare.com`, never a staging
  host.
- Never commit keys. The MCP Registry signing key stays with whoever publishes.

## Checking changes

Validate the plugin files against their schemas before committing:

```bash
curl -s -o /tmp/plugin.schema.json https://agent-plugins.org/schemas/1.0.0/plugin.schema.json
curl -s -o /tmp/mcp.schema.json https://agent-plugins.org/schemas/1.0.0/mcp.schema.json
npx ajv-cli@5 validate --spec=draft2020 -s /tmp/plugin.schema.json -d plugin.json
npx ajv-cli@5 validate --spec=draft2020 -s /tmp/mcp.schema.json -d mcp.json
```

`mcp-publisher publish` validates `server.json` against the registry schema
itself. Then confirm the server still answers:

```bash
curl -s -H 'Content-Type: application/json' -H 'Accept: application/json, text/event-stream' \
  https://answeringservicecare.com/mcp -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```
