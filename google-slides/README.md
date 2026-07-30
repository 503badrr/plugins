# Google Slides

Cursor plugin that connects agents to [Google Slides](https://slides.google.com) through Google's remote [Model Context Protocol](https://modelcontextprotocol.io/) server.

Read Google Slides presentations and apply updates in the signed-in Workspace account.

## Install

1. Open **Cursor Settings → Plugins**.
2. Search for **Google Slides**.
3. Click **Install**, then complete the Google sign-in prompt.

Or run `/add-plugin google-slides` in chat.

## MCP

```json
{
  "mcpServers": {
    "google-slides": {
      "type": "http",
      "url": "https://slidesmcp.googleapis.com/mcp/v1"
    }
  }
}
```

Auth is OAuth 2.0 against Google. Cursor prompts for Google sign-in when the plugin connects.

## Docs

- Google MCP setup: https://developers.google.com/workspace/slides/api/guides/configure-mcp-server
- Workspace MCP overview: https://developers.google.com/workspace/guides/configure-mcp-servers

## License

MIT
