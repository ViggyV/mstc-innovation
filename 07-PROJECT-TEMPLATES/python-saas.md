# Template: Python SaaS Application

Copy this to your project's `CLAUDE.md` and customize.

```markdown
# [Product Name] — Claude Code Agent

## PROJECT OVERVIEW
**What:** [SaaS product description]
**Goal:** [Primary business objective]
**Users:** [Target users]
**Stage:** [MVP | Growth | Scale]

## TECH STACK
- **Backend:** FastAPI + Python 3.12
- **Database:** PostgreSQL + SQLAlchemy 2.0
- **Cache:** Redis
- **Queue:** Celery + Redis
- **Auth:** JWT + OAuth2
- **Frontend:** Next.js 14 + TypeScript + Tailwind CSS
- **Hosting:** AWS (ECS + RDS + ElastiCache)
- **CI/CD:** GitHub Actions

## ARCHITECTURE
```
src/
├── api/
│   ├── routes/          # FastAPI route modules
│   ├── deps.py          # Dependency injection
│   └── middleware.py     # Custom middleware
├── core/
│   ├── config.py        # Settings (pydantic-settings)
│   ├── security.py      # Auth utilities
│   └── exceptions.py    # Custom exceptions
├── domain/
│   ├── models/          # SQLAlchemy models
│   ├── schemas/         # Pydantic schemas
│   └── services/        # Business logic
├── infrastructure/
│   ├── database.py      # DB session management
│   ├── cache.py         # Redis client
│   └── repositories/    # Data access layer
└── workers/
    └── tasks.py         # Celery tasks
```

## CONVENTIONS
- Repository pattern for all DB access
- Pydantic v2 for all request/response schemas
- Alembic for migrations (never edit applied migrations)
- Structured logging with structlog
- All endpoints require authentication unless explicitly public

## CONSTRAINTS
- Never expose internal error details in API responses
- Always use parameterized queries
- All new endpoints need OpenAPI docs and tests
- Rate limit all public endpoints
```
