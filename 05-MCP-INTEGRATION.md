# 05 - MCP Integration

MCP (Model Context Protocol) extends Claude Code with **external tool access** — browsers, databases, APIs, file systems, and custom services.

## What MCP Does

```
┌─────────────────────────────────────────────────────┐
│                   CLAUDE CODE                        │
│                                                      │
│  Built-in Tools        MCP Tools (External)          │
│  ┌──────────┐         ┌────────────────────┐        │
│  │ Read     │         │ browser_navigate   │        │
│  │ Write    │         │ browser_click      │        │
│  │ Edit     │         │ browser_snapshot   │        │
│  │ Bash     │         │ database_query     │        │
│  │ Glob     │         │ slack_send_message │        │
│  │ Grep     │         │ github_create_pr   │        │
│  │ Agent    │         │ [your_custom_tool] │        │
│  └──────────┘         └────────────────────┘        │
│       ▲                        ▲                     │
│       │                        │                     │
│   Built into              Connected via              │
│   Claude Code             MCP Protocol               │
└─────────────────────────────────────────────────────┘
```

## Configuration

MCP servers are configured in `~/.claude/.mcp.json` (global) or `<project>/.claude/.mcp.json` (project-level):

```json
{
  "mcpServers": {
    "browser": {
      "command": "npx",
      "args": ["@anthropic/mcp-browser"],
      "env": {}
    },
    "postgres": {
      "command": "npx",
      "args": ["@anthropic/mcp-postgres", "postgresql://localhost:5432/mydb"],
      "env": {}
    },
    "filesystem": {
      "command": "npx",
      "args": ["@anthropic/mcp-filesystem", "/path/to/allowed/dir"],
      "env": {}
    },
    "custom-api": {
      "command": "python",
      "args": ["./mcp-servers/my-api-server.py"],
      "env": {
        "API_KEY": "your-key-here"
      }
    }
  }
}
```

## Common MCP Servers

### Browser Automation
```json
{
  "browser": {
    "command": "npx",
    "args": ["@anthropic/mcp-browser"]
  }
}
```
**Tools provided:** `browser_navigate`, `browser_click`, `browser_type`, `browser_snapshot`, `browser_take_screenshot`

### Database Access
```json
{
  "postgres": {
    "command": "npx",
    "args": ["@anthropic/mcp-postgres", "postgresql://user:pass@host:5432/db"]
  }
}
```
**Tools provided:** `query`, `list_tables`, `describe_table`

### File System
```json
{
  "filesystem": {
    "command": "npx",
    "args": ["@anthropic/mcp-filesystem", "/allowed/path"]
  }
}
```
**Tools provided:** `read_file`, `write_file`, `list_directory`, `search_files`

### Memory / Knowledge Graph
```json
{
  "memory": {
    "command": "npx",
    "args": ["@anthropic/mcp-memory"]
  }
}
```
**Tools provided:** `create_entities`, `search_entities`, `create_relations`

## Building Custom MCP Servers

### Python (FastMCP)

```python
# my_server.py
from fastmcp import FastMCP

mcp = FastMCP("My Custom Server")

@mcp.tool()
def search_inventory(query: str, category: str = "all") -> str:
    """Search the product inventory.

    Args:
        query: Search terms
        category: Product category filter
    """
    # Your implementation here
    results = db.search(query, category=category)
    return json.dumps(results)

@mcp.tool()
def create_order(product_id: str, quantity: int) -> str:
    """Create a new order.

    Args:
        product_id: The product to order
        quantity: Number of units
    """
    order = db.create_order(product_id, quantity)
    return f"Order {order.id} created successfully"

@mcp.resource("inventory://stats")
def get_inventory_stats() -> str:
    """Get current inventory statistics."""
    stats = db.get_stats()
    return json.dumps(stats)

if __name__ == "__main__":
    mcp.run()
```

### TypeScript (MCP SDK)

```typescript
// server.ts
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

const server = new McpServer({ name: "my-server", version: "1.0.0" });

server.tool(
  "search_docs",
  { query: z.string(), limit: z.number().optional().default(10) },
  async ({ query, limit }) => {
    const results = await searchDocuments(query, limit);
    return { content: [{ type: "text", text: JSON.stringify(results) }] };
  }
);

const transport = new StdioServerTransport();
await server.connect(transport);
```

## MCP in Your Agent Architecture

### Pattern: Domain-Specific MCP Server

Create an MCP server that wraps your project's specific APIs:

```python
# For a restaurant management system:
@mcp.tool()
def get_daily_sales(date: str, location: str) -> str:
    """Get sales data for a specific date and location."""

@mcp.tool()
def forecast_demand(days_ahead: int, product: str) -> str:
    """Forecast product demand for the next N days."""

@mcp.tool()
def check_inventory(location: str) -> str:
    """Check current inventory levels at a location."""
```

### Pattern: Multi-Server Orchestration

```json
{
  "mcpServers": {
    "crm": { "command": "python", "args": ["./mcp/crm-server.py"] },
    "analytics": { "command": "python", "args": ["./mcp/analytics-server.py"] },
    "email": { "command": "python", "args": ["./mcp/email-server.py"] },
    "browser": { "command": "npx", "args": ["@anthropic/mcp-browser"] }
  }
}
```

Claude Code can then use tools from ALL servers in a single conversation:
```
1. Search CRM for customer → crm.search_customers
2. Pull their analytics → analytics.get_customer_metrics
3. Draft email → (Claude generates)
4. Send email → email.send_message
```

## Discovering MCP Capabilities

Use the `mcp-find` tool to search for available MCP servers:

```
mcp-find query="browser"     → finds browser automation servers
mcp-find query="database"    → finds database connectors
mcp-find query="slack"       → finds Slack integration servers
```

Then add them:
```
mcp-add name="browser" activate=true
```

## Security Considerations

1. **Environment variables**: Store API keys in `env` field, never hardcode
2. **Path restrictions**: Use filesystem MCP with explicit allowed paths
3. **Query safety**: Database MCP servers should use read-only connections where possible
4. **Network access**: Be aware of what external services MCP servers can reach
5. **Tool approval**: Users must approve MCP tool calls (unless pre-approved in skills)
