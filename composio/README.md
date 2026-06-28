# composio

A Claude Tag plugin that declares the hosted **Composio** MCP server and bundles
a skill for using it well.

## What's inside

```
composio/
|-- .claude-plugin/
|   `-- plugin.json                 # plugin manifest
|-- .mcp.json                       # declares the Composio MCP server
`-- skills/
    `-- composio-tools/
        `-- SKILL.md                # how Claude should use Composio tools
```

## What it does

The `.mcp.json` points Claude at `https://connect.composio.dev/mcp`, Composio's
unified endpoint for 1000+ app integrations (Gmail, Slack, Linear, Notion,
GitHub, calendars, CRMs, and more).

## Important: the plugin only *declares* the server

The plugin tells Claude the server exists. It does **not** carry the credential.
In Claude Tag you add the credential separately on the Access bundle's
**Credentials** tab. Note that `connect.composio.dev/mcp` authenticates with a
**custom header**, not `Authorization: Bearer` -- a Bearer token bounces you to
the OAuth login screen. Set it up as:

- **Credential type:** Bearer (only to expose the Custom headers fields)
- **Allowed websites:** `connect.composio.dev`
- **Custom header** -> Name `x-consumer-api-key`, Prefix *(blank)*, Value `ck_...`
  (your Composio API key from dashboard.composio.dev)
- Delete the default `Authorization` / `Bearer` row

Use a **dedicated** Composio API key for the agent (not a personal login) so its
activity is separately auditable and revocable.
