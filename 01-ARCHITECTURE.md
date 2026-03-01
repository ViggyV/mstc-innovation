# 01 - Claude Code Agent Architecture

## How Claude Code Agents Work

Claude Code is not just a chatbot. It's an **autonomous agent** with a layered instruction system, persistent memory, tool access, and skill discovery. Understanding the architecture lets you build agents that are far more capable than basic prompting.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    CLAUDE CODE AGENT STACK                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌───────────────────────────────────────────────────────────────┐  │
│  │                    INSTRUCTION LAYER                          │  │
│  │  CLAUDE.md (global) + CLAUDE.md (project) + CLAUDE.md (local)│  │
│  │  These define WHO the agent is and HOW it behaves            │  │
│  └──────────────────────────┬────────────────────────────────────┘  │
│                              │                                      │
│  ┌───────────────────────────▼──────────────────────────────────┐  │
│  │                     SKILLS LAYER                              │  │
│  │  Auto-activating domain expertise (SKILL.md files)           │  │
│  │  Triggered by context — user never has to invoke them        │  │
│  └──────────────────────────┬────────────────────────────────────┘  │
│                              │                                      │
│  ┌───────────────────────────▼──────────────────────────────────┐  │
│  │                   COMMANDS LAYER                              │  │
│  │  /slash-commands = user-invoked workflows                    │  │
│  │  Stored as .md files in ~/.claude/commands/                  │  │
│  └──────────────────────────┬────────────────────────────────────┘  │
│                              │                                      │
│  ┌───────────────────────────▼──────────────────────────────────┐  │
│  │                     TOOLS LAYER                               │  │
│  │  Built-in: Read, Write, Edit, Bash, Glob, Grep, Agent       │  │
│  │  External: MCP servers (browser, DB, APIs, custom)           │  │
│  └──────────────────────────┬────────────────────────────────────┘  │
│                              │                                      │
│  ┌───────────────────────────▼──────────────────────────────────┐  │
│  │                    MEMORY LAYER                               │  │
│  │  ~/.claude/projects/<project-hash>/memory/                   │  │
│  │  Persistent knowledge across sessions                        │  │
│  └──────────────────────────────────────────────────────────────┘  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## The File Hierarchy

Claude Code reads instructions from multiple levels, each overriding the previous:

```
Priority (highest to lowest):
─────────────────────────────

1. ~/.claude/CLAUDE.md              ← Global (all projects)
2. <project>/.claude/CLAUDE.md      ← Project-level (this repo)
3. <project>/CLAUDE.md              ← Project root (team-shared)
4. <subfolder>/CLAUDE.md            ← Directory-scoped instructions
```

### What Goes Where

| Level | Use For | Example |
|-------|---------|---------|
| **Global** `~/.claude/CLAUDE.md` | Personal preferences, universal rules | "Always use TypeScript", "Never auto-commit" |
| **Project** `.claude/CLAUDE.md` | Project architecture, conventions | Tech stack, file structure, API patterns |
| **Root** `CLAUDE.md` | Team-shared instructions (committed to git) | Coding standards, PR guidelines |
| **Subfolder** `src/api/CLAUDE.md` | Directory-specific context | "All files here are REST endpoints" |

## Key Directories

```
~/.claude/
├── CLAUDE.md                          # Global instructions
├── commands/                          # Slash commands (*.md files)
│   ├── agent-designer.md             # /agent-designer
│   ├── code-reviewer.md              # /code-reviewer
│   └── ...
├── skills/                            # Auto-activating skills
│   └── my-skill/
│       └── SKILL.md                   # Skill definition
├── projects/
│   └── <project-hash>/
│       └── memory/
│           └── MEMORY.md              # Persistent memory for this project
└── .mcp.json                          # MCP server configuration
```

## Agent Execution Flow

When you send a message to Claude Code:

```
1. User Message
   │
   ▼
2. Load CLAUDE.md chain (global → project → local)
   │
   ▼
3. Check available skills → auto-activate matching ones
   │
   ▼
4. Parse intent → route to appropriate tool(s)
   │
   ▼
5. Execute tools (Read, Edit, Bash, MCP, etc.)
   │  ├── Can spawn sub-agents for parallel work
   │  ├── Can use MCP tools for external services
   │  └── Can read/write persistent memory
   │
   ▼
6. Return results → continue agentic loop if needed
   │
   ▼
7. Final response to user
```

## Sub-Agent System

Claude Code can spawn specialized sub-agents for parallel work:

| Agent Type | Purpose | Tools Available |
|------------|---------|-----------------|
| `general-purpose` | Complex multi-step tasks | All tools |
| `Explore` | Fast codebase search | Read-only tools |
| `Plan` | Architecture planning | Read-only tools |
| `claude-code-guide` | Claude Code documentation | Web + read tools |

Sub-agents run in isolation and return results to the main agent. This keeps the main context window clean.

## Permission Model

Tools have three permission levels:

1. **Auto-allowed**: Read, Glob, Grep (safe, read-only)
2. **Prompt-required**: Edit, Write, Bash (user must approve)
3. **Configurable**: Per-skill `allowed-tools` in SKILL.md frontmatter

You can pre-approve tools in your CLAUDE.md:
```markdown
<!-- In CLAUDE.md -->
When running tests, use `pytest` without asking for confirmation.
```

Or in skill frontmatter:
```yaml
allowed-tools:
  - Bash(pytest*)
  - Bash(npm test*)
```
