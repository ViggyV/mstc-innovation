# PROMPT: Build a Production AI Agent with the Anthropic SDK

> Copy everything below the line into Claude Code. Replace the bracketed placeholders.

---

Build a production-ready AI agent using the Anthropic Python SDK.

## AGENT SPEC

**Purpose**: [WHAT THE AGENT DOES]
**Tools it needs**: [LIST EACH TOOL AND WHAT IT DOES]
**Safety rules**: [WHAT THE AGENT MUST NEVER DO]
**Max iterations**: [HOW MANY REASONING STEPS BEFORE STOPPING]
**Model**: claude-sonnet-4-20250514

## GENERATE THIS STRUCTURE

```
agent/
├── agent.py              # Main ReAct agent loop
├── tools.py              # Tool definitions and implementations
├── guardrails.py         # Safety checks and rate limiting
├── config.py             # Configuration (model, params, limits)
├── prompts.py            # System prompts and templates
├── memory.py             # Conversation history management
├── logger.py             # Structured logging
├── server.py             # FastAPI wrapper for HTTP access
├── tests/
│   ├── test_agent.py     # Agent loop tests
│   ├── test_tools.py     # Individual tool tests
│   ├── test_guardrails.py # Safety tests
│   └── test_e2e.py       # End-to-end scenario tests
├── pyproject.toml         # Dependencies
├── Dockerfile             # Container deployment
└── README.md              # Setup and usage guide
```

## REQUIREMENTS FOR EACH FILE

### agent.py
```python
# ReAct loop with:
# - Conversation history management
# - Tool call detection and execution
# - Stop condition (end_turn OR max iterations)
# - Error recovery (tool failures don't crash the loop)
# - Streaming support (optional)
# - Structured result return (not just text)
```

### tools.py
```python
# For each tool:
# - JSON schema definition (name, description, input_schema)
# - Implementation function with type hints
# - Error handling that returns useful messages
# - Input validation before execution

# Tool registry pattern:
# TOOLS = [schema1, schema2, ...]
# HANDLERS = {"tool_name": handler_fn, ...}
```

### guardrails.py
```python
# Safety layer wrapping tool execution:
# - Tool allowlist (only registered tools can run)
# - Rate limiting (max N tool calls per session)
# - Pattern blocking (regex patterns for dangerous operations)
# - Audit logging (every tool call logged with args and result)
# - Cost tracking (token usage per request)
```

### config.py
```python
# Pydantic settings:
# - ANTHROPIC_API_KEY (from env)
# - MODEL (default: claude-sonnet-4-20250514)
# - MAX_TOKENS (default: 4096)
# - MAX_ITERATIONS (default: 15)
# - TEMPERATURE (default: 0)
# - ALLOWED_TOOLS (list)
# - MAX_TOOL_CALLS (rate limit)
```

### server.py
```python
# FastAPI endpoints:
# POST /agent/run — Run agent with task, return result
# POST /agent/stream — SSE streaming agent execution
# GET /agent/health — Health check
# GET /agent/tools — List available tools
```

### tests/
```python
# Test categories:
# 1. Unit: Each tool works correctly
# 2. Integration: Agent uses tools correctly in sequence
# 3. Safety: Guardrails block forbidden operations
# 4. Edge: Max iterations, tool failures, empty responses
# 5. E2E: Complete task scenarios
```

## IMPLEMENTATION GUIDELINES

- Use `anthropic` SDK (latest version)
- Type hints on everything
- Async where appropriate (API calls, I/O)
- No global state — all state in agent instance
- Dependency injection for testability
- Structured logging with `structlog`
- Environment-based configuration

Generate all files with complete, working implementations. No stubs or placeholders.
