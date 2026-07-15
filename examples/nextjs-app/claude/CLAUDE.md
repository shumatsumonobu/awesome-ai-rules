# Next.js Fullstack App

Monorepo with a Next.js frontend and a shared API package. TypeScript, Tailwind CSS, Drizzle ORM.

## Tech Stack

- Next.js 15 (App Router), React 19, TypeScript 5
- Tailwind CSS v4, Drizzle ORM, PostgreSQL
- Vitest + React Testing Library
- pnpm workspaces

## Workspace Layout

```
packages/
  web/        # Next.js frontend (see its own CLAUDE.md)
  api/        # Backend logic + Drizzle schema (see its own CLAUDE.md)
  shared/     # Shared types and utilities
```

## Build & Test

```sh
pnpm install
pnpm dev              # Start all packages
pnpm build            # Production build
pnpm test             # Run all tests
pnpm typecheck        # tsc --noEmit across workspaces
```

## Conventions

- Server Components by default. Add `"use client"` only at the smallest leaf that needs it.
- Named exports only. No default exports.
- Use `@/` path aliases within each package.
- Validate external input with Zod.

## Don't

- Don't use the Pages Router.
- Don't use `useEffect` for data fetching.
- Don't install axios -- use native `fetch`.
- Don't commit `.env` files.
