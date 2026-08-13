# AgentMail

Cursor plugin that connects agents to [AgentMail](https://agentmail.to) through AgentMail's official hosted [Model Context Protocol](https://modelcontextprotocol.io/) server.

Give agents their own inboxes: create addresses, send and receive mail, search threads, and manage drafts and attachments.

## Install

1. Open **Cursor Settings → Plugins**.
2. Search for **AgentMail**.
3. Click **Install**, then set your AgentMail API key (below).

Or run `/add-plugin agentmail` in chat.

## MCP

```json
{
  "mcpServers": {
    "agentmail": {
      "type": "http",
      "url": "https://mcp.agentmail.to/mcp",
      "headers": {
        "x-api-key": "${AGENTMAIL_API_KEY}"
      }
    }
  }
}
```

## What agents can do

| Category | Capabilities |
| --- | --- |
| Inboxes | List, get, create, update, and delete agent inboxes |
| Threads | List, search, get, update, and delete conversations |
| Messages | List, search, send, reply, forward, and update messages; fetch attachments |
| Drafts | Create, list, get, update, send, and delete drafts |

The hosted runtime is the source of truth for tool names and schemas. Call `list_inboxes` as a read-only smoke test after connecting.

## Setup

No credential ships with this plugin — it carries only a `${AGENTMAIL_API_KEY}` placeholder, and each install supplies its own key.

1. Open the [AgentMail console](https://console.agentmail.to).
2. Go to **Settings → API Keys** and create a key.
3. Copy the full value, including the `am_` prefix.
4. In **Dashboard → Plugins → Configure**, set **AgentMail API key** to that value.

Tool calls run with that key's organization, pod, and inbox scope. Rotate or revoke the key from the console if it is ever exposed.

Prefer the `x-api-key` header (what this plugin ships). Query-string keys end up in logs and history. AgentMail also accepts `Authorization: Bearer <am_...>` if a client requires that form.

## Docs

- AgentMail MCP (Cursor API key): https://www.agentmail.to/docs/integrations/mcp#cursor-api-key
- Hosted server: https://mcp.agentmail.to/mcp
- Console: https://console.agentmail.to
- MCP implementation: https://github.com/agentmail-to/agentmail-mcp

Logo is AgentMail's official GitHub organization mark.

## License

MIT
