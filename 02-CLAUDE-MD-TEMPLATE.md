# 02 - CLAUDE.md Template

The `CLAUDE.md` file is the **brain** of your agent. It tells Claude Code who it is, what it's building, and how to behave. This is the single most important file in your agent configuration.

## Template Structure

Every great CLAUDE.md follows this structure:

```
1. Project Identity     → What is this project?
2. Tech Stack           → What tools/frameworks are we using?
3. Architecture         → How is the code organized?
4. Conventions          → Coding standards and patterns
5. Priorities           → What to work on and in what order
6. Constraints          → What NOT to do
7. Workflow Rules       → How to approach tasks
```

---

## Full Template

Copy and customize this for your project:

```markdown
# [PROJECT NAME] — Claude Code Agent Configuration

## PROJECT OVERVIEW

**What:** [One-line description of the project]
**Goal:** [Primary objective — what does success look like?]
**Users:** [Who uses this? Developers? End users? Both?]
**Stage:** [Prototype | MVP | Production | Maintenance]

## TECH STACK

### Core
- **Language:** [Python 3.12 | TypeScript 5.x | Go 1.22 | etc.]
- **Framework:** [FastAPI | Next.js 14 | Django | Express | etc.]
- **Database:** [PostgreSQL | MongoDB | Supabase | etc.]
- **ORM/Query:** [SQLAlchemy | Prisma | Drizzle | etc.]

### Frontend (if applicable)
- **UI Framework:** [React 18 | Vue 3 | Svelte | etc.]
- **Styling:** [Tailwind CSS | shadcn/ui | styled-components | etc.]
- **State:** [Zustand | Redux Toolkit | Jotai | etc.]

### Infrastructure
- **Hosting:** [Vercel | AWS | GCP | Railway | etc.]
- **CI/CD:** [GitHub Actions | GitLab CI | etc.]
- **Monitoring:** [Sentry | DataDog | etc.]

### AI/ML (if applicable)
- **LLM Provider:** [Anthropic Claude | OpenAI | etc.]
- **Frameworks:** [LangChain | LlamaIndex | etc.]
- **Vector DB:** [Pinecone | Chroma | pgvector | etc.]

## ARCHITECTURE

### Directory Structure
```
project-root/
├── src/
│   ├── api/           # API routes and controllers
│   ├── core/          # Business logic and domain models
│   ├── db/            # Database schemas, migrations, repositories
│   ├── services/      # External service integrations
│   └── utils/         # Shared utilities
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── scripts/           # Build, deploy, seed scripts
├── docs/              # Documentation
└── config/            # Environment and app configuration
```

### Key Files
- `src/core/config.py` — All configuration and environment variables
- `src/db/models.py` — Database schema definitions
- `src/api/routes/` — API endpoint definitions
- `tests/conftest.py` — Shared test fixtures

### Data Flow
```
[Client] → [API Layer] → [Service Layer] → [Repository Layer] → [Database]
                ↓                ↓
          [Validation]    [Business Logic]
```

## CONVENTIONS

### Code Style
- Use [language-specific formatter: ruff | prettier | gofmt]
- Max line length: [100 | 120] characters
- Use type hints / TypeScript strict mode everywhere
- Prefer composition over inheritance
- Prefer small, focused functions (< 30 lines)

### Naming
- Files: `snake_case.py` | `kebab-case.ts`
- Classes: `PascalCase`
- Functions: `snake_case` | `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- Database tables: `snake_case`, plural (`users`, `orders`)

### API Design
- RESTful endpoints: `GET /api/v1/resources`
- Use Pydantic/Zod for request/response validation
- Always return consistent error format:
  ```json
  {"error": {"code": "NOT_FOUND", "message": "Resource not found"}}
  ```

### Error Handling
- Use custom exception classes, not generic exceptions
- Log errors with structured context (user_id, request_id)
- Never expose internal errors to API consumers

### Testing
- Write tests for all new features
- Use [pytest | vitest | jest] with [coverage tool]
- Minimum coverage: [80% | 90%]
- Test naming: `test_<function>_<scenario>_<expected>`

## PRIORITIES

### Current Sprint
1. **[Feature/Task Name]** — [Brief description]
   - Key files: `src/...`, `tests/...`
   - Acceptance criteria: [What "done" looks like]

2. **[Feature/Task Name]** — [Brief description]
   - Key files: `src/...`
   - Acceptance criteria: [What "done" looks like]

### Backlog
- [ ] [Future task 1]
- [ ] [Future task 2]
- [ ] [Future task 3]

## CONSTRAINTS

### Never Do
- Never commit `.env` files or secrets
- Never use `sudo` or run destructive commands without asking
- Never modify database migrations that have been applied to production
- Never skip tests to "save time"
- Never use `any` type in TypeScript (use `unknown` if needed)

### Always Do
- Always read a file before editing it
- Always run tests after making changes
- Always use environment variables for configuration
- Always validate user input at API boundaries
- Always handle errors gracefully

### Security
- Sanitize all user input
- Use parameterized queries (never string concatenation for SQL)
- Implement rate limiting on public endpoints
- Use HTTPS everywhere
- Follow OWASP Top 10 guidelines

## WORKFLOW RULES

### When Starting a Task
1. Read the relevant files first
2. Understand existing patterns before adding new code
3. Plan the approach for non-trivial changes
4. Write tests alongside implementation

### When Modifying Code
1. Prefer editing existing files over creating new ones
2. Follow existing patterns in the codebase
3. Keep changes minimal and focused
4. Don't refactor unrelated code

### When Something Breaks
1. Read the error message carefully
2. Check recent changes that might have caused it
3. Don't retry the same failed approach
4. Ask the user if the path forward is unclear

### Git Workflow
- Branch naming: `feature/description`, `fix/description`
- Commit messages: imperative mood, < 72 chars
- One logical change per commit
- Never force-push to main/develop
```

---

## Examples by Project Type

### Python Data Science Project
```markdown
## TECH STACK
- **Language:** Python 3.12
- **Data:** pandas, polars, numpy
- **Visualization:** plotly, matplotlib, seaborn
- **ML:** scikit-learn, xgboost, prophet
- **Notebooks:** Jupyter Lab
- **Environment:** conda / pyproject.toml

## CONVENTIONS
- Use type hints on all function signatures
- Docstrings on all public functions (numpy style)
- Data pipelines: raw → processed → features → model
- Save all artifacts to `outputs/` with timestamps
```

### Next.js SaaS Application
```markdown
## TECH STACK
- **Framework:** Next.js 14 (App Router)
- **Language:** TypeScript 5.x (strict mode)
- **Styling:** Tailwind CSS + shadcn/ui
- **Database:** Supabase (PostgreSQL)
- **Auth:** Supabase Auth + RLS
- **Payments:** Stripe

## CONVENTIONS
- Server Components by default, Client Components only when needed
- All data fetching in Server Components or Route Handlers
- Use Zod for all form validation
- Co-locate components with their pages
```

### Python API Service
```markdown
## TECH STACK
- **Framework:** FastAPI
- **Database:** PostgreSQL + SQLAlchemy 2.0
- **Cache:** Redis
- **Queue:** Celery
- **Docs:** Auto-generated OpenAPI

## CONVENTIONS
- Dependency injection via FastAPI Depends()
- Repository pattern for all database access
- Pydantic v2 models for all request/response schemas
- Alembic for database migrations
```

### Mobile App (React Native)
```markdown
## TECH STACK
- **Framework:** React Native + Expo
- **Language:** TypeScript
- **Navigation:** React Navigation 6
- **State:** Zustand + React Query
- **Backend:** Firebase / Supabase

## CONVENTIONS
- Use Expo modules when available
- Screen components in screens/, shared in components/
- Use React Query for all server state
- Test on both iOS and Android before completing
```
