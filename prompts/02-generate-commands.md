# PROMPT: Generate Complete Command Library

> Copy everything below the line into Claude Code to generate commands for your workflow.

---

Generate a complete Claude Code slash command library tailored to my development workflow.

## MY CONTEXT

- **Project type**: [web app / API / mobile / data / etc.]
- **Language**: [Python / TypeScript / Go / etc.]
- **Framework**: [Next.js / FastAPI / Django / etc.]
- **Test framework**: [pytest / vitest / jest / etc.]
- **CI/CD**: [GitHub Actions / GitLab CI / etc.]
- **Team size**: [solo / small team / large team]

## GENERATE THESE COMMANDS

Create each as a `.md` file in `~/.claude/commands/`:

### Development Commands

**`[project]-review.md`** — Code review tailored to our conventions
- Check our specific linting rules
- Verify our architectural patterns
- Flag our known anti-patterns
- Score: critical / major / minor / nit

**`[project]-test.md`** — Generate tests in our style
- Use our test framework and fixtures
- Follow our test naming convention
- Cover: happy path, edge cases, error cases
- Include setup/teardown matching our patterns

**`[project]-feature.md`** — New feature implementation guide
- Our file creation checklist
- Required files (route, model, test, migration, etc.)
- Integration points to update
- Documentation requirements

**`[project]-debug.md`** — Debug workflow for our architecture
- Log locations for our system
- Common error patterns and fixes
- Database debugging queries
- Performance profiling for our stack

### Git Commands

**`smart-commit.md`** — Analyze changes and create semantic commit
- Read staged diff
- Categorize: feat / fix / refactor / docs / test / chore
- Generate message following our commit convention
- Include scope based on changed files

**`pr-create.md`** — Create PR with our template
- Generate title from branch name and commits
- Fill our PR template sections
- Add appropriate labels
- List reviewers based on changed files

### Quality Commands

**`security-scan.md`** — Project-specific security review
- Check for our common vulnerability patterns
- Verify auth is applied correctly
- Check for exposed secrets or PII
- Validate input sanitization

**`performance-check.md`** — Performance analysis
- Check for N+1 queries in our ORM
- Verify pagination on list endpoints
- Check bundle size impact (frontend)
- Identify missing indexes

### Documentation Commands

**`api-doc.md`** — Generate API documentation
- Read route definitions
- Generate request/response examples
- Document error codes
- Create curl/httpie examples

**`changelog.md`** — Generate changelog from commits
- Group by: features, fixes, breaking changes
- Include PR references
- Tag with version number

## FORMAT

Each command file should follow this structure:
```markdown
# [Command Name]

You are an expert at [specific expertise].

## Activation
This skill activates when the user needs help with:
- [trigger 1]
- [trigger 2]

## Process
### 1. [Step Name]
[What to do]

### 2. [Step Name]
[What to do]

## Output Format
Provide:
1. [deliverable]
2. [deliverable]
```

Generate all commands now. After creating each file, tell me the command name and how to invoke it.
