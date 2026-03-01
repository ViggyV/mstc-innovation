# Template: Next.js Full-Stack Application

Copy this to your project's `CLAUDE.md` and customize.

```markdown
# [App Name] — Claude Code Agent

## PROJECT OVERVIEW
**What:** [Application description]
**Goal:** [Primary objective]
**Users:** [Target users]
**Stage:** [Prototype | MVP | Production]

## TECH STACK
- **Framework:** Next.js 14 (App Router)
- **Language:** TypeScript 5.x (strict mode)
- **Styling:** Tailwind CSS + shadcn/ui
- **Database:** Supabase (PostgreSQL + Auth + Storage)
- **ORM:** Drizzle ORM
- **State:** Zustand (client) + React Query (server)
- **Forms:** React Hook Form + Zod
- **Testing:** Vitest + Playwright
- **Hosting:** Vercel

## ARCHITECTURE
```
app/
├── (auth)/              # Auth-required layout group
│   ├── dashboard/
│   ├── settings/
│   └── layout.tsx
├── (public)/            # Public layout group
│   ├── page.tsx         # Landing page
│   └── pricing/
├── api/                 # Route handlers
│   └── v1/
└── layout.tsx           # Root layout

components/
├── ui/                  # shadcn/ui components
├── forms/               # Form components
├── layouts/             # Layout components
└── features/            # Feature-specific components

lib/
├── db/                  # Database client + schemas
├── auth/                # Auth utilities
├── api/                 # API client functions
└── utils/               # Shared utilities
```

## CONVENTIONS
- Server Components by default
- Client Components only for interactivity (use "use client")
- All data fetching in Server Components or Route Handlers
- Zod schemas for all form + API validation
- Co-locate components with their feature
- Use Next.js Image, Link, and Font components
- Environment variables: NEXT_PUBLIC_ prefix for client-side only

## CONSTRAINTS
- Never use `any` type — use `unknown` if truly needed
- Never fetch data in Client Components (use React Query)
- Always handle loading and error states
- All forms need client-side AND server-side validation
- Images must use next/image for optimization
```
