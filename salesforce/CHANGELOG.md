# Changelog

All notable changes to this plugin will be documented here.

## 1.0.0 — initial release

- Added the `salesforce` MCP server backed by Salesforce Hosted MCP.
- Declared `SALESFORCE_MCP_URL` and `CLIENT_ID` plugin variables so each org can point at its own server and External Client App.
- Pinned OAuth scopes to `mcp_api` and `refresh_token`.
