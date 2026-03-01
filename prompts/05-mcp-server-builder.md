# PROMPT: Build a Custom MCP Server

> Copy everything below the line into Claude Code. Replace the bracketed placeholders.

---

Build a custom MCP (Model Context Protocol) server that gives Claude Code access to [SERVICE NAME].

## SERVICE DETAILS

**Service**: [NAME OF THE SERVICE/API]
**Base URL**: [API BASE URL]
**Auth**: [API_KEY / OAUTH / JWT / NONE]
**Key operations I need**:
1. [OPERATION 1]: [WHAT IT DOES]
2. [OPERATION 2]: [WHAT IT DOES]
3. [OPERATION 3]: [WHAT IT DOES]
4. [OPERATION 4]: [WHAT IT DOES]
5. [OPERATION 5]: [WHAT IT DOES]

## GENERATE

### 1. MCP Server (Python with FastMCP)

```python
# server.py — Complete MCP server implementation
#
# Requirements:
# - Each operation as a @mcp.tool() with clear docstring
# - Proper error handling (API errors → helpful messages)
# - Input validation on all parameters
# - Response formatting (return structured data Claude can use)
# - Environment variables for secrets (never hardcode)
# - Rate limiting to respect API limits
# - Retry logic with exponential backoff
```

### 2. Configuration

Generate `.claude/.mcp.json` entry:
```json
{
  "mcpServers": {
    "[service-name]": {
      "command": "python",
      "args": ["path/to/server.py"],
      "env": {
        "API_KEY": "[placeholder]"
      }
    }
  }
}
```

### 3. Test Script

```python
# test_server.py — Verify each tool works
# For each tool:
# - Call with valid input → verify response format
# - Call with invalid input → verify error handling
# - Call with missing auth → verify helpful error message
```

### 4. README

- What this MCP server does
- How to set it up (install deps, set env vars)
- Available tools with examples
- Troubleshooting

## QUALITY REQUIREMENTS

- Every tool docstring should be clear enough for Claude to understand WHEN to use it
- Parameters should have descriptions that explain valid values
- Error messages should suggest how to fix the issue
- The server should handle API being down gracefully
- All secrets via environment variables

Generate the complete, working implementation.
