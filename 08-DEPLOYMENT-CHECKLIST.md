# 08 - Agent Deployment Checklist

Step-by-step checklist for setting up a Claude Code agent for a new project.

## Phase 1: Foundation

### CLAUDE.md Setup
- [ ] Create `~/.claude/CLAUDE.md` (global preferences — do once)
- [ ] Create `<project>/CLAUDE.md` or `<project>/.claude/CLAUDE.md`
- [ ] Define: Project overview, tech stack, architecture
- [ ] Define: Coding conventions, naming patterns
- [ ] Define: Current priorities and tasks
- [ ] Define: Constraints (never do / always do)
- [ ] Define: Workflow rules

### Verify It Works
```bash
cd your-project
claude
# Ask: "What project am I working on?"
# Agent should describe your project from CLAUDE.md
```

## Phase 2: Skills

### Identify Domain Skills Needed
- [ ] What domains does this project touch? (backend, frontend, data, etc.)
- [ ] Any specialized frameworks? (FastAPI, Next.js, PyTorch, etc.)
- [ ] Any workflows that should auto-activate? (testing, security, etc.)

### Create Skills
- [ ] Create `~/.claude/skills/<skill-name>/SKILL.md` for each domain
- [ ] Write clear `description` field (this controls auto-activation)
- [ ] Add code templates and patterns
- [ ] Define step-by-step processes
- [ ] Set `allowed-tools` for pre-approved operations

### Verify Skills Activate
```bash
claude
# Work on a task in the skill's domain
# Check if the agent follows the skill's process
```

## Phase 3: Commands

### Identify Repeatable Workflows
- [ ] What tasks does the team do repeatedly?
- [ ] What workflows need consistency?
- [ ] What tasks would benefit from a checklist?

### Create Commands
- [ ] Create `~/.claude/commands/<command-name>.md` for each workflow
- [ ] Follow the standard template (Activation → Process → Output Format)
- [ ] Test each command: `/command-name`

### Essential Commands for Most Projects
- [ ] `/code-review` — Review code quality
- [ ] `/test-gen` — Generate tests
- [ ] `/debug` — Systematic debugging
- [ ] `/commit` — Create good commits

## Phase 4: MCP Integration (Optional)

### Identify External Tools Needed
- [ ] Browser automation? → `@anthropic/mcp-browser`
- [ ] Database access? → `@anthropic/mcp-postgres`
- [ ] Custom APIs? → Build custom MCP server

### Configure MCP
- [ ] Create `~/.claude/.mcp.json` or `<project>/.claude/.mcp.json`
- [ ] Add server configurations
- [ ] Test each MCP tool individually
- [ ] Verify tools appear in Claude Code

## Phase 5: Memory

### Set Up Project Memory
- [ ] Create initial `MEMORY.md` with project context
- [ ] Document key decisions made
- [ ] Record patterns discovered
- [ ] Note user preferences

### Memory Maintenance
- [ ] Review memory files periodically
- [ ] Remove outdated information
- [ ] Add insights from completed work
- [ ] Keep `MEMORY.md` under 200 lines

## Phase 6: Team Sharing

### What to Commit (shared with team)
```
<project>/
├── CLAUDE.md                  # Team coding standards
├── .claude/
│   └── .mcp.json             # MCP config (no secrets!)
└── scripts/                   # Supporting scripts
```

### What Stays Personal (not committed)
```
~/.claude/
├── CLAUDE.md                  # Personal preferences
├── commands/                  # Personal command library
├── skills/                    # Personal skill library
└── projects/<hash>/memory/    # Project-specific memory
```

## Quick Reference Card

```
┌────────────────────────────────────────────────┐
│           AGENT SETUP QUICK REFERENCE          │
├────────────────────────────────────────────────┤
│                                                 │
│  1. CLAUDE.md   → WHO the agent is              │
│  2. Skills      → WHAT it's expert at           │
│  3. Commands    → HOW to trigger workflows      │
│  4. MCP         → WHAT tools it can use         │
│  5. Memory      → WHAT it remembers             │
│                                                 │
│  Files:                                         │
│  ~/.claude/CLAUDE.md ......... Global rules      │
│  <project>/CLAUDE.md ......... Project rules     │
│  ~/.claude/skills/*/SKILL.md . Auto-activate     │
│  ~/.claude/commands/*.md ..... /slash-commands    │
│  ~/.claude/.mcp.json ......... External tools    │
│  ~/.claude/projects/*/memory/  Persistent memory │
│                                                 │
└────────────────────────────────────────────────┘
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Agent ignores CLAUDE.md | Check file path is correct, verify with "What project am I working on?" |
| Skill doesn't activate | Check `description` field matches the task context |
| Command not found | Verify file is in `~/.claude/commands/` with `.md` extension |
| MCP tool fails | Check server process is running, verify config in `.mcp.json` |
| Memory not persisting | Check `~/.claude/projects/<hash>/memory/` exists |
| Agent uses wrong patterns | Add more specific instructions in CLAUDE.md constraints section |
