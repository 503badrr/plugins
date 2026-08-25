# Coda

Cursor plugin that connects agents to [Coda](https://coda.io) through Coda's official remote [Model Context Protocol](https://modelcontextprotocol.io/) server.

Search and read Coda docs, pages, and tables, and create or update pages and rows with the same access the signed-in user has.

## Install

1. Open **Cursor Settings → Plugins**.
2. Search for **Coda**.
3. Click **Install**, then complete the Coda sign-in prompt.

Or run `/add-plugin coda` in chat.

## MCP

```json
{
  "mcpServers": {
    "coda": {
      "type": "http",
      "url": "https://coda.io/apis/mcp"
    }
  }
}
```

Auth is OAuth 2 with PKCE. Cursor prompts for Coda sign-in when the plugin connects. An OAuth connection is automatically scoped to both read and write.

## Before you connect

Coda MCP is in beta. Doc Makers on paid Coda plans get the full experience; Doc Makers on free plans and Editors get a capped "free taste" (30 requests per week, 60 per month), and Editors are limited to read tools.

## What agents can do

| Category | Capabilities |
| --- | --- |
| Search | Search across docs and pages |
| Docs & pages | Read, create, and update docs and pages |
| Tables & rows | Read and write table rows |
| Helpers | `url_decode` turns a pasted Coda URL into the IDs the other tools need |

The hosted runtime is the source of truth for tool names and schemas.

## Notes

- Tool calls run as the Coda user who authorizes the connection.
- Coda also accepts a personal access token sent as `Authorization: Bearer <token>`, which lets you pick read-only, write-only, or read+write. The token must be created with restriction type **MCP** or the server returns 401. Coda currently recommends the token path for Cursor because of refresh-token handling.
- Coda is rebranding to Superhuman Docs and serves the same server at `https://docs.superhuman.com/apis/mcp`. Coda says existing `coda.io` connections keep working.
- The `coda-mcp` npm package is a community local server by a third-party maintainer, unrelated to this hosted endpoint.

## Docs

- Connect to the Coda MCP: https://help.coda.io/hc/en-us/articles/44722661982989-Connect-to-the-Coda-MCP
- Superhuman Docs MCP: https://help.superhuman.com/hc/en-us/articles/46210076980365-Connect-to-the-Coda-MCP
- Server URL: https://coda.io/apis/mcp

Logo is Coda's official mark, from the `coda` GitHub organization.

## License

MIT
