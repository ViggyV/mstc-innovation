# 03 - Skills System

Skills are **auto-activating expertise modules** that Claude Code loads based on context. Unlike slash commands (which users invoke manually), skills activate automatically when the agent detects a relevant task.

## Skill vs Command vs CLAUDE.md

| Feature | CLAUDE.md | Skill (SKILL.md) | Command (*.md) |
|---------|-----------|-------------------|-----------------|
| Activation | Always loaded | Auto-activated by context | User invokes with `/name` |
| Scope | Project-wide | Domain-specific | Single workflow |
| Location | Root / .claude/ | ~/.claude/skills/<name>/ | ~/.claude/commands/ |
| Loaded | Every session | When relevant | On demand |
| Best for | Project rules | Domain expertise | Repeatable workflows |

## Creating a Skill

### Minimal Skill

```
~/.claude/skills/my-skill/
└── SKILL.md
```

### SKILL.md Anatomy

```markdown
---
name: my-skill
description: >
  Expert at [domain]. Use when the user needs help with [trigger conditions].
  Activates for [specific tasks].
license: MIT
allowed-tools:
  - Bash(pytest*)
  - Bash(npm test*)
metadata:
  author: your-name
  version: "1.0"
---

# My Skill Name

You are an expert at [domain] with deep knowledge of [specifics].

## Activation

This skill activates when the user needs help with:
- [Trigger condition 1]
- [Trigger condition 2]
- [Trigger condition 3]

## Process

### 1. Assessment
Ask about:
- [Required context 1]
- [Required context 2]

### 2. Implementation

[Step-by-step process the agent should follow]

### 3. Patterns & Templates

[Code templates, architecture diagrams, reference implementations]

### 4. Validation

[How to verify the work is correct]

## Output Format

Provide:
1. [Deliverable 1]
2. [Deliverable 2]
3. [Deliverable 3]
```

### Frontmatter Reference

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Hyphen-case, must match folder name |
| `description` | Yes | When to activate — Claude reads this to decide |
| `license` | No | License name or file reference |
| `allowed-tools` | No | Pre-approved tool patterns |
| `metadata` | No | Custom key-value pairs |

## Skill Design Patterns

### Pattern 1: Domain Expert

For deep expertise in a specific technology or domain.

```markdown
---
name: fastapi-expert
description: >
  Expert at building FastAPI applications. Activates when working with
  FastAPI routes, Pydantic models, dependency injection, or async endpoints.
---

# FastAPI Expert

You are a FastAPI expert with deep knowledge of async Python, Pydantic v2,
dependency injection, and OpenAPI documentation.

## Activation
- Creating or modifying FastAPI routes
- Working with Pydantic models
- Setting up middleware or dependencies
- Configuring CORS, auth, or rate limiting

## Patterns

### Route Template
[code examples]

### Dependency Injection
[code examples]

### Error Handling
[code examples]
```

### Pattern 2: Workflow Orchestrator

For multi-step processes that follow a specific sequence.

```markdown
---
name: database-migration
description: >
  Manages database schema changes safely. Activates when creating migrations,
  modifying database models, or deploying schema changes.
---

# Database Migration Workflow

## Process (MUST follow in order)

### Step 1: Analyze Current Schema
- Read current models
- Check pending migrations
- Identify what's changing

### Step 2: Generate Migration
- Create migration file
- Review SQL that will be generated
- Check for data loss risks

### Step 3: Test Migration
- Run on test database
- Verify rollback works
- Check data integrity

### Step 4: Apply
- Apply to staging first
- Verify application behavior
- Then apply to production
```

### Pattern 3: Quality Gate

For enforcing standards and catching issues.

```markdown
---
name: pre-commit-review
description: >
  Reviews code quality before commits. Activates when the user is about
  to commit or asks for a code review.
---

# Pre-Commit Review

## Checklist (ALL must pass)

### Security
- [ ] No hardcoded secrets or API keys
- [ ] Input validation on all endpoints
- [ ] SQL injection protection

### Quality
- [ ] All new functions have tests
- [ ] No TODO/FIXME without tracking issue
- [ ] Error handling is appropriate

### Performance
- [ ] No N+1 query patterns
- [ ] Large datasets use pagination
- [ ] Async operations where appropriate
```

### Pattern 4: Project Orchestrator (Parent Skill)

For complex projects with multiple sub-domains.

```markdown
---
name: my-project-core
description: >
  Master orchestration skill for [Project Name]. Use when building,
  designing, or working on any aspect of this system. Coordinates
  all child skills and provides architecture guidance.
---

# [Project Name] — Core Agent

## Child Skills
This skill coordinates:
- `frontend-ui` — React/Next.js components
- `backend-api` — FastAPI endpoints
- `data-pipeline` — ETL and data processing
- `deployment` — CI/CD and infrastructure

## Architecture Decision Flow
1. Identify which domain(s) are affected
2. Load relevant child skill(s)
3. Follow domain-specific patterns
4. Validate against project-wide constraints

## Cross-Cutting Concerns
- All APIs use JWT authentication
- All database access goes through repository layer
- All errors follow standard error format
- All new features need tests
```

## Complex Skill with Resources

Skills can include additional files:

```
~/.claude/skills/api-builder/
├── SKILL.md                    # Main skill definition
├── templates/
│   ├── route-template.py       # Code templates
│   ├── model-template.py
│   └── test-template.py
├── references/
│   ├── api-patterns.md         # Reference documentation
│   └── error-codes.md
└── scripts/
    ├── generate-route.sh       # Automation scripts
    └── validate-schema.sh
```

The SKILL.md can reference these files:

```markdown
## Templates

When creating a new API route, use the template at `templates/route-template.py`.
When adding a new model, follow `templates/model-template.py`.

## Reference

See `references/api-patterns.md` for approved patterns.
See `references/error-codes.md` for the error code registry.
```

## Skill Discovery

Claude Code discovers skills through:

1. **Directory scanning**: Checks `~/.claude/skills/` for `SKILL.md` files
2. **Description matching**: Reads the `description` field to determine relevance
3. **Context analysis**: Matches current task against skill activation triggers
4. **Auto-loading**: Loads matching skills into the conversation context

Write your `description` field carefully — it's the primary trigger for activation.

## Tips for Effective Skills

1. **Be specific in descriptions** — vague descriptions lead to wrong activations
2. **Include concrete examples** — code templates are more useful than abstract advice
3. **Define clear processes** — step-by-step workflows prevent the agent from improvising
4. **Set boundaries** — define what the skill does NOT handle
5. **Keep skills focused** — one skill per domain, use parent skills to orchestrate
6. **Test with real tasks** — verify the skill activates when expected
