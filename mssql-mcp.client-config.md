# mssql-mcp — client configuration

The MCP server runs on PDAPP2 and connects to SQL Server as the **`mate`** login.
Point any MCP-capable app at the HTTP endpoint below — no SQL credentials are
needed on the client side (the server holds them).

- **Endpoint:** `http://pdapp2.inhat.hu:8006/mcp`
- **Transport:** streamable HTTP
- **Effective SQL identity:** `mate`
- Requires network access to `pdapp2.inhat.hu:8006`.

## Claude Code (project or user scope)
`.mcp.json` (already added to this repo root):
```json
{
  "mcpServers": {
    "mssql-mcp": { "type": "http", "url": "http://pdapp2.inhat.hu:8006/mcp" }
  }
}
```
Or add it via CLI:
```
claude mcp add --transport http mssql-mcp http://pdapp2.inhat.hu:8006/mcp
```

## Claude Desktop
Add to `claude_desktop_config.json`
(Windows: `%APPDATA%\Claude\claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "mssql-mcp": { "type": "http", "url": "http://pdapp2.inhat.hu:8006/mcp" }
  }
}
```

## Generic MCP client
Use transport = HTTP (streamable), URL = `http://pdapp2.inhat.hu:8006/mcp`.

Health/config without an MCP client: `curl http://pdapp2.inhat.hu:8006/info`.

Tools exposed: `execute_sql`, `describe_table`, `list_tables`, `list_schemas`,
`schema_discovery`, `get_database_info`, `get_policy_info`, `check_db_connection`.
Writes are currently enabled. See the `mssql-mcp-usage` skill for conventions.
