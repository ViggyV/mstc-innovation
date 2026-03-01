# PROMPT: Generate Project-Specific Skills

> Copy everything below the line into Claude Code in your target project directory.

---

Analyze this codebase and create a complete Claude Code skill library. Each skill should auto-activate when Claude encounters a task in that domain.

## INSTRUCTIONS

1. **Identify domains**: What are the 3-6 major domains in this codebase? (e.g., API layer, database layer, frontend components, auth system, payment processing, etc.)

2. **For each domain, create a SKILL.md** at `~/.claude/skills/[project]-[domain]/SKILL.md` with:

```yaml
---
name: [project]-[domain]
description: >
  [Precise description of when this skill should activate.
  Include specific file patterns, keywords, and task types.
  This is what Claude reads to decide activation — be specific.]
allowed-tools:
  - Bash([relevant test commands]*)
---
```

Followed by markdown body:
```markdown
# [Domain] Expert for [Project]

You are an expert at [domain] within this project.

## Activation
This skill activates when:
- Working with files in [specific directories]
- Tasks involving [specific operations]
- User mentions [specific keywords]

## Architecture
[How this domain is structured in this project]

## Patterns
[3-5 code patterns with complete examples from the actual codebase]

## Anti-Patterns
[What NOT to do, based on the codebase conventions]

## Checklist
When completing work in this domain:
- [ ] [Quality check 1]
- [ ] [Quality check 2]
- [ ] [Quality check 3]
```

3. **Create a parent/orchestrator skill** at `~/.claude/skills/[project]-core/SKILL.md` that:
   - Lists all child skills
   - Defines cross-cutting concerns (auth, logging, error handling)
   - Provides architecture decision guidance
   - Routes to appropriate child skills

4. **Verify each skill** by describing a hypothetical task and confirming the right skill would activate based on the description field.

## OUTPUT

For each skill created, tell me:
- Skill name
- File path
- What triggers it
- What it provides
