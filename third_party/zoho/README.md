# Zoho

Cursor plugin that connects agents to [Zoho](https://www.zoho.com) through Zoho's official remote [Model Context Protocol](https://modelcontextprotocol.io/) server.

Drive Zoho CRM, Desk, Books, Projects, Mail, Cliq, WorkDrive, and the rest of the Zoho suite from one MCP server that you assemble yourself, picking exactly which tools to expose.

## Install

1. Open **Cursor Settings → Plugins**.
2. Search for **Zoho**.
3. Click **Install**, then set your Zoho MCP server URL (below).

Or run `/add-plugin zoho` in chat.

## MCP

```json
{
  "mcpServers": {
    "zoho": {
      "type": "http",
      "url": "${ZOHO_MCP_SERVER_URL}"
    }
  }
}
```

Zoho MCP does not have a single shared endpoint. You build a server in the Zoho MCP console, choose which apps and tools it exposes, and Zoho generates a URL for it — so this plugin takes that URL as a setting.

Authorization is OAuth 2.1 on top of the URL. On the first tool call Zoho opens a browser prompt to authorize the connection.

## Before you connect

1. Sign in to the Zoho MCP console for **your account's data-center region**: `https://mcp.zoho.com` (US), `https://mcp.zoho.in` (India), `https://mcp.zoho.eu` (Europe), `https://mcp.zoho.com.au` (Australia), or `https://mcp.zoho.jp` (Japan). Using the wrong region returns a 403.
2. Click **Create MCP Server**, or start from a pre-configured template.
3. Under **Add Tools**, pick the Zoho apps and the individual tools you want exposed — CRM, Desk, Books, and Projects can all live on one server.
4. Choose an authorization mode. **Authorization on Demand** has each user authenticate with their own Zoho account; **Authorization via Connection** has a Super Admin authorize once and share the server.
5. Open **Connect** in the sidebar, copy the generated server URL, and set it in **Dashboard → Plugins → Configure**.

The generated URL embeds an API key, so anyone holding it can call your tools. Keep it out of shared configs and repositories, and use **Regenerate API Key** in the console if it leaks.

## What agents can do

| Category | Capabilities |
| --- | --- |
| CRM | Query pipelines, create and update leads and deals, customize modules and fields, and configure workflow rules |
| Desk | Search and triage tickets, reassign and update statuses, and send replies |
| Books | Raise invoices, record and categorize expenses, reconcile banking, and read financial reports |
| Projects | Create and update projects, tasks, milestones, and bugs, and log time |
| Other Zoho apps | Mail, Calendar, Cliq, WorkDrive, Analytics, Billing, Inventory, Invoice, Expense, Assist, and more |
| Third-party apps | Zoho MCP can also expose connectors for 500+ non-Zoho apps on the same server |

The hosted runtime is the source of truth for tool names and schemas.

## Notes

- The tool catalog is whatever you selected in the console — narrow the server there rather than trying to constrain the agent.
- Tool calls respect the authorizing user's role-based permissions in each Zoho app, and draw on that app's existing API limits.
- Zoho CRM also provisions pre-built MCP servers from **Setup → Developer Hub → MCP for AI Agents**, with one-click install into Cursor. Those URLs work here too.
- Some Zoho pages show a `npx mcp-remote` config for Cursor. That is not needed — Cursor connects to the URL directly and detects the transport.
- Zoho MCP is free as of now, and Zoho has committed to giving notice before introducing pricing.

## Docs

- Zoho MCP: https://www.zoho.com/mcp/
- Zoho MCP setup, regions, and hosts: https://www.zoho.com/analytics/api/v2/zoho-analytics-mcp-server/remote-mcp/zoho-mcp.html
- Connect Zoho MCP to Cursor: https://www.zoho.com/mail/help/mcp/mcp-cursor.html
- Zoho CRM MCP: Cursor setup: https://www.zoho.com/crm/developer/docs/mcp/setup/cursor.html
- Zoho Books MCP: https://www.zoho.com/us/books/help/mcp/zoho-books-mcp.html
- Zoho Projects MCP: https://www.zoho.com/projects/mcp.html

Logo is Zoho's official mark, from the `zoho` GitHub organization.

## License

MIT
