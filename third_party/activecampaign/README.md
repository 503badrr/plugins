# ActiveCampaign

Cursor plugin that connects agents to [ActiveCampaign](https://www.activecampaign.com) through ActiveCampaign's official remote [Model Context Protocol](https://modelcontextprotocol.io/) server.

Read and update ActiveCampaign contacts, lists, tags, and custom fields, and work with campaigns, automations, and CRM deals.

## Install

1. Open **Cursor Settings → Plugins**.
2. Search for **ActiveCampaign**.
3. Click **Install**, then set your ActiveCampaign Remote MCP URL (below).

Or run `/add-plugin activecampaign` in chat.

## MCP

```json
{
  "mcpServers": {
    "activecampaign": {
      "type": "http",
      "url": "${ACTIVECAMPAIGN_MCP_URL}"
    }
  }
}
```

ActiveCampaign gives every account its own Remote MCP URL rather than publishing a shared endpoint, so this plugin takes that URL as a setting. Authorization is OAuth — Cursor prompts for ActiveCampaign sign-in when the plugin connects.

## Before you connect

1. In ActiveCampaign, click the **Settings** gear in the upper right and choose **Developer**.
2. Copy your **Remote MCP URL**. It is unique to your account.
3. Set it in **Dashboard → Plugins → Configure**, then complete the ActiveCampaign login when Cursor prompts.

If the Remote MCP URL option is missing from Developer settings, check that **ActiveCampaign branding** is turned on for the account — ActiveCampaign gates the option behind it.

## What agents can do

| Category | Capabilities |
| --- | --- |
| Contacts | Read and update contacts, lists, tags, and custom fields |
| Campaigns | Inspect campaigns and their performance |
| Automations | List automations, and add or remove contacts from them |
| CRM | Deals and pipeline records |

The hosted runtime is the source of truth for tool names and schemas.

## Notes

- Tool calls run as the ActiveCampaign user who authorizes the connection.
- There is no shared `mcp.activecampaign.com` endpoint. Aggregator sites publish one, and it contradicts ActiveCampaign's own documentation — use the URL from your Developer settings.
- ActiveCampaign also publishes an official local server on PyPI as `ac-mcp-server`, which authenticates with an API key instead. The remote server is the supported path for AI clients.
- The OAuth handshake has been reported as flaky on some clients. If the first authorization attempt fails, retry before assuming a misconfiguration.

## Docs

- ActiveCampaign Remote MCP Server: https://developers.activecampaign.com/page/mcp
- Get started with the MCP server: https://help.activecampaign.com/hc/en-us/articles/22566179229596-Get-started-with-the-ActiveCampaign-MCP-Server
- MCP server announcement: https://www.activecampaign.com/blog/mcp-server

Logo is ActiveCampaign's official mark, from the `ActiveCampaign` GitHub organization.

## License

MIT
