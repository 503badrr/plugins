# Clay

Connect Cursor to [Clay](https://www.clay.com) via Clay's hosted MCP server. Find and enrich people and companies across 150+ data providers, run AI research agents, and trigger your team's approved Clay workflows — directly from Cursor.

## What you can do

- **Find people and companies** — search Clay's data universe with natural-language criteria.
- **Enrich records** — pull emails, phone numbers, firmographics, technographics, and more across 150+ providers.
- **Run AI research agents** — Claygent answers open-ended research questions about accounts and contacts.
- **Trigger workflows** — kick off Clay tables and workflows your team has approved for MCP access.

## Installation

1. Install this plugin from the Cursor marketplace.
2. When Cursor connects to the Clay MCP server for the first time, your browser opens to sign in at `app.clay.com`.
3. Approve the connection. That's it — no API keys or client IDs to copy.

Clay's MCP server supports OAuth 2.1 with Dynamic Client Registration (DCR), so Cursor registers itself automatically and completes the standard PKCE authorization flow.

## Requirements

- A Clay account with access to a workspace.
- Your Clay workspace admin may need to allow MCP client connections (Clay workspace Settings → MCP). If sign-in succeeds but tools fail, check with your admin.

## MCP server

| Server | URL | Auth |
| ------ | --- | ---- |
| `clay` | `https://api.clay.com/v3/mcp` | OAuth 2.1 (DCR + PKCE), handled automatically by Cursor |

## For workspace admins

- Connections made through this plugin appear in your Clay workspace's MCP client list, labeled with the Cursor client name.
- Access is scoped to what the signed-in Clay user can do; workflow triggers are limited to workflows approved for MCP access.
- To revoke access, remove the client from your workspace's MCP settings in Clay.

## Support

- Clay MCP docs: <https://university.clay.com/docs/connect-to-clay-mcp>
- Issues with this plugin: <https://github.com/cursor/plugins/issues>
