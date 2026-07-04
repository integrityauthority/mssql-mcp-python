# mssql-mcp — client configuration

The MCP server runs on PDAPP2. Point any MCP-capable app at the HTTP endpoint.

- **Endpoint:** `http://pdapp2.inhat.hu:8006/mcp`
- **Transport:** streamable HTTP
- **Default SQL identity:** `srv_vambery` (scoped service account) — used when you
  send no credential headers.
- Requires network access to `pdapp2.inhat.hu:8006`.

## Option A — use the default identity (`srv_vambery`)
No credentials needed on the client. `.mcp.json` (already in this repo root):
```json
{
  "mcpServers": {
    "mssql-mcp": { "type": "http", "url": "http://pdapp2.inhat.hu:8006/mcp" }
  }
}
```

## Option B — authenticate as your own login (e.g. `mate`)
Send your SQL credentials as headers in the client config. This overrides the
server default **only for your connection** — no server change needed. Fill in
your own password; do NOT commit a file containing a real password.
```json
{
  "mcpServers": {
    "mssql-mcp-mate": {
      "type": "http",
      "url": "http://pdapp2.inhat.hu:8006/mcp",
      "headers": {
        "X-MSSQL-User": "mate",
        "X-MSSQL-Password": "<your-mate-password>"
      }
    }
  }
}
```
Optional header: `X-MSSQL-Trusted-Connection: true` for Windows/Integrated auth
(ignores user/password). Works from any MCP client that supports custom headers
(Claude Code, Claude Desktop, generic HTTP MCP clients) — reuse the same block.

> Security: the endpoint is plain HTTP, so credentials travel unencrypted on the
> network. Prefer a trusted network / put HTTPS in front for production use.
> Your access is bounded by that login's own SQL Server permissions.

## Claude Desktop
Add the same `mcpServers` block to `claude_desktop_config.json`
(Windows: `%APPDATA%\Claude\claude_desktop_config.json`).

## Verify
- Config/health without a client: `curl http://pdapp2.inhat.hu:8006/info`.
- Which login am I? call `execute_sql` with `SELECT SUSER_SNAME()`.

Tools exposed: `execute_sql`, `describe_table`, `list_tables`, `list_schemas`,
`schema_discovery`, `get_database_info`, `get_policy_info`, `check_db_connection`.
Writes are enabled (bounded by your login's permissions). See the
`mssql-mcp-usage` skill for conventions.
