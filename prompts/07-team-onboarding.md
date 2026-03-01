# PROMPT: Set Up Claude Code Agent for a Development Team

> Copy everything below the line into Claude Code. This sets up a shared team agent configuration.

---

Set up Claude Code for our development team so everyone has a consistent, powerful agent experience.

## TEAM CONTEXT

**Team size**: [NUMBER]
**Project**: [DESCRIPTION]
**Stack**: [TECH STACK]
**Repo**: [GIT URL OR LOCAL PATH]
**Workflow**: [AGILE / KANBAN / TRUNK-BASED / GITFLOW]

## GENERATE SHARED CONFIGURATION (committed to repo)

### 1. Project CLAUDE.md

Create `CLAUDE.md` at the repo root. This is the team-shared agent brain.

Include:
- Project overview that any new team member can understand
- Complete tech stack with versions
- Architecture diagram (ASCII) showing how components connect
- Directory structure with purpose of each folder
- Coding conventions based on existing linter configs and code patterns
- Git workflow (branch naming, commit format, PR process)
- Testing requirements (what needs tests, coverage expectations)
- Common commands (build, test, lint, deploy)
- Constraints (security rules, architectural boundaries)

### 2. Team .claude/ Directory

Create `.claude/` directory in the repo with:

**`.claude/commands/`** — Team commands committed to repo:

- `review.md` — Code review using our standards
- `test.md` — Test generation using our framework
- `feature.md` — New feature checklist
- `debug.md` — Debugging workflow for our stack
- `deploy.md` — Deployment checklist

**`.claude/.mcp.json`** — Shared MCP config (no secrets):
```json
{
  "mcpServers": {
    // Only servers that don't require personal credentials
  }
}
```

### 3. Personal Setup Guide

Create `.claude/SETUP.md` with instructions for each team member:

```markdown
# Claude Code Setup for [Project]

## Step 1: Global Configuration
Add these to your ~/.claude/CLAUDE.md:
[Personal preferences that complement the project config]

## Step 2: Personal Commands (optional)
Copy any commands you want from ~/.claude/commands/

## Step 3: MCP Servers (if needed)
Add personal API keys to your local .claude/.mcp.json:
[Template with placeholders for personal credentials]

## Step 4: Verify Setup
Run these commands to verify everything works:
1. `claude` → Ask "What project am I working on?" → Should describe our project
2. `/review` → Should produce code review in our format
3. `/test` → Should generate tests using our framework

## Common Issues
- [Issue 1]: [Fix]
- [Issue 2]: [Fix]
```

## GENERATE PERSONAL CONFIGURATION TEMPLATES (not committed)

### 4. Personal ~/.claude/CLAUDE.md Addition

```markdown
## [PROJECT] PERSONAL PREFERENCES
[Template for personal coding preferences that work within team conventions]
```

### 5. Recommended Personal Commands

Commands that individual developers might want:
- `my-standup.md` — Generate standup summary from recent git activity
- `my-todos.md` — Extract TODOs from files I've changed recently
- `my-review-prep.md` — Self-review before requesting PR review

## VERIFY

After creating everything:
1. Confirm CLAUDE.md accurately describes the project
2. Test each team command
3. Verify no secrets are in committed files
4. Ensure .gitignore excludes personal configs

Output a summary of everything created and where it lives.
