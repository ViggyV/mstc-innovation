# 04 - Slash Commands

Slash commands are **user-invoked workflows** triggered by typing `/command-name` in Claude Code. They're markdown files stored in `~/.claude/commands/` that inject specialized instructions into the conversation.

## How Commands Work

```
User types: /code-reviewer
                │
                ▼
Claude loads: ~/.claude/commands/code-reviewer.md
                │
                ▼
The markdown content becomes the agent's instructions for this task
                │
                ▼
Agent executes the workflow defined in the command
```

## Creating a Command

### 1. Create a markdown file

```bash
# File: ~/.claude/commands/my-command.md
```

### 2. Write the command definition

Every command file follows this structure:

```markdown
# Command Name

You are an expert at [domain/task].

## Activation

This skill activates when the user needs help with:
- [Trigger 1]
- [Trigger 2]
- [Trigger 3]

## Process

### 1. [First Step Name]
Ask about / Do:
- [Action or question]
- [Action or question]

### 2. [Second Step Name]
[Implementation details, templates, code examples]

### 3. [Third Step Name]
[Validation, output formatting]

## Output Format

Provide:
1. [Deliverable 1]
2. [Deliverable 2]
3. [Deliverable 3]
```

## Command Categories & Examples

### Development Commands

**`/code-reviewer`** — Systematic code review
```markdown
# Code Reviewer

You are a senior engineer performing a thorough code review.

## Process

### 1. Scope
- Read the changed files
- Understand the intent of the changes

### 2. Review Checklist
- [ ] Logic correctness
- [ ] Edge cases handled
- [ ] Error handling appropriate
- [ ] No security vulnerabilities
- [ ] Tests cover new behavior
- [ ] Performance considerations
- [ ] Naming is clear and consistent

### 3. Feedback Format
For each issue found:
- **File:Line** — [severity: critical/major/minor/nit]
- **Issue:** What's wrong
- **Suggestion:** How to fix it
```

**`/test-generator`** — Generate tests for code
```markdown
# Test Generator

Generate comprehensive tests for the specified code.

## Process
1. Read the source file
2. Identify all public functions/methods
3. For each, generate tests covering:
   - Happy path
   - Edge cases (empty, null, boundary values)
   - Error conditions
4. Use the project's testing framework
5. Follow existing test patterns in the codebase

## Output
- Test file with all cases
- Run the tests to verify they pass
```

**`/debug-assistant`** — Systematic debugging
```markdown
# Debug Assistant

Systematically debug the reported issue.

## Process
1. **Reproduce**: Understand the symptoms
2. **Isolate**: Find the minimal reproduction
3. **Trace**: Follow the data flow to the root cause
4. **Fix**: Apply the minimal correct fix
5. **Verify**: Confirm the fix works and nothing else broke
6. **Prevent**: Add a test that would catch this regression
```

### Content & Communication Commands

**`/meeting-recap`** — Summarize meeting notes
```markdown
# Meeting Recap

Transform raw meeting notes into a structured summary.

## Output Format
### Meeting: [Title]
**Date:** [Date]
**Attendees:** [Names]

### Key Decisions
- [Decision 1]
- [Decision 2]

### Action Items
| Owner | Task | Due Date |
|-------|------|----------|
| [Name] | [Task] | [Date] |

### Discussion Notes
[Brief summary of key topics discussed]

### Next Steps
[What happens next]
```

**`/linkedin-post`** — Draft LinkedIn content
```markdown
# LinkedIn Post Creator

Create engaging LinkedIn content from a topic or experience.

## Process
1. Ask about the topic, key insight, and target audience
2. Draft using the hook-story-insight-CTA framework:
   - **Hook** (first line): Pattern interrupt or bold claim
   - **Story** (2-3 paragraphs): Personal experience or case study
   - **Insight** (1 paragraph): The lesson or framework
   - **CTA** (last line): Question or invitation to engage
3. Keep under 1300 characters for optimal reach
4. Use line breaks for readability
5. No hashtags in the body (add 3-5 at the end)
```

### Data & Analysis Commands

**`/data-validator`** — Validate datasets
```markdown
# Data Validator

Perform comprehensive data quality checks.

## Checks
1. **Schema**: Column types, nullable fields, constraints
2. **Completeness**: Missing values, empty strings
3. **Consistency**: Date formats, category values, ranges
4. **Uniqueness**: Duplicate records, unique key violations
5. **Freshness**: Data recency, timestamp gaps
6. **Relationships**: Foreign key integrity, orphaned records

## Output
- Quality score (0-100)
- Issues found with severity
- Recommended fixes
```

### Git & DevOps Commands

**`/commit-helper`** — Write good commit messages
```markdown
# Commit Helper

Analyze staged changes and create a proper commit.

## Process
1. Run `git diff --staged` to see changes
2. Categorize: feature | fix | refactor | docs | test | chore
3. Write commit message:
   - Subject: imperative mood, < 72 chars, explains WHY
   - Body: details of WHAT changed (if needed)
4. Stage any unstaged related files
5. Create the commit
```

## Naming Conventions

| Convention | Example | Invoked As |
|------------|---------|------------|
| Hyphen-case | `code-reviewer.md` | `/code-reviewer` |
| Single word | `commit.md` | `/commit` |
| Namespaced | `superpowers:brainstorm.md` | `/superpowers:brainstorm` |

## Commands with Arguments

Commands can accept arguments from the user:

```
User: /test-generator src/api/routes.py
```

The argument (`src/api/routes.py`) is passed as context to the command. Reference it in your command:

```markdown
# Test Generator

Generate tests for the file specified by the user.
Read the provided file path and analyze all exported functions.
```

## Commands vs Skills: When to Use Which

| Use a **Command** when... | Use a **Skill** when... |
|---------------------------|------------------------|
| User explicitly triggers it | Should auto-activate by context |
| It's a discrete workflow | It's ongoing domain expertise |
| Has a clear start and end | Applies throughout a session |
| Different each time it runs | Same knowledge every time |

**Examples:**
- `/deploy` = Command (explicit action)
- `fastapi-expert` = Skill (auto-activates when writing FastAPI code)
- `/code-review` = Command (user requests review)
- `security-audit` = Skill (auto-checks for vulnerabilities)

## Building a Command Library

Organize commands by category in `~/.claude/commands/`:

```
~/.claude/commands/
├── # AI & ML
│   ├── agent-designer.md
│   ├── chatbot-creator.md
│   ├── prompt-engineer.md
│   ├── rag-builder.md
│   └── model-trainer.md
├── # Development
│   ├── code-reviewer.md
│   ├── test-generator.md
│   ├── debug-assistant.md
│   └── code-refactor.md
├── # Git & DevOps
│   ├── commit-helper.md
│   ├── pr-reviewer.md
│   ├── ci-cd-builder.md
│   └── docker-composer.md
├── # Content
│   ├── linkedin-post.md
│   ├── meeting-recap.md
│   └── email-polisher.md
└── # Data
    ├── data-validator.md
    ├── pandas-expert.md
    └── analytics-builder.md
```
