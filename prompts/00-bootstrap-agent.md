# PROMPT: Bootstrap a Claude Code Agent for This Project

> Copy everything below the line into Claude Code in your target project directory.

---

You are setting up a Claude Code agent for this project. Perform a complete codebase analysis and generate production-ready agent configuration files.

## PHASE 1: CODEBASE ANALYSIS

Scan the entire project and gather:

1. **Package manifests**: Read package.json, pyproject.toml, Cargo.toml, go.mod, Gemfile, pom.xml — whatever exists
2. **Directory structure**: Map all top-level and second-level directories with their purposes
3. **Source patterns**: Read 5-8 representative source files to identify coding patterns (naming, error handling, architecture)
4. **Config files**: Read .eslintrc, tsconfig.json, ruff.toml, .prettierrc, Dockerfile, docker-compose.yml, CI configs
5. **Tests**: Identify testing framework, test file patterns, fixtures/helpers
6. **Documentation**: Read README, CONTRIBUTING, any docs/ folder

## PHASE 2: GENERATE CLAUDE.md

Create a `CLAUDE.md` at the project root with ALL of these sections:

```
# [Project Name] — Claude Code Agent

## PROJECT OVERVIEW
What, Goal, Users, Stage (from what you found)

## TECH STACK
Every dependency, framework, tool detected — categorized

## ARCHITECTURE
Directory map with purpose of each folder/file group

## KEY FILES
The 10-15 most important files with one-line descriptions

## DATA FLOW
How data moves through the system (request → processing → storage → response)

## CONVENTIONS
- Code style (from linter configs and source patterns)
- Naming patterns (from actual variable/function/file names)
- Error handling patterns (from source code)
- Testing patterns (from test files)
- Import organization (from source code)

## CONSTRAINTS
### Never Do
[5+ rules based on security, data safety, architectural boundaries]

### Always Do
[5+ rules based on observed best practices]

## WORKFLOW RULES
### When Adding a New Feature
[Step by step based on existing patterns]

### When Fixing a Bug
[Step by step]

### When Modifying Existing Code
[Step by step]

### Git Workflow
[Based on branch names, commit history, CI config]
```

## PHASE 3: GENERATE COMMANDS

Create 3 project-specific slash commands in `~/.claude/commands/`:

1. **[project]-review.md** — Code review command tailored to this project's patterns
2. **[project]-test.md** — Test generation command using this project's test framework
3. **[project]-debug.md** — Debug workflow command for this project's architecture

## PHASE 4: VERIFY

After creating everything:
1. Read back the CLAUDE.md and verify accuracy
2. Confirm each command file exists
3. List any gaps or recommendations for improvement

Be ruthlessly specific. Every line in CLAUDE.md should be based on evidence from the actual codebase — no generic advice.
