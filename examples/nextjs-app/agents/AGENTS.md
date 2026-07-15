# AGENTS.md

This file is read by OpenAI Codex, GitHub Copilot, AND Gemini simultaneously -- one file, three editors. If your team uses different AI editors, AGENTS.md is the shared baseline. Add editor-specific files (CLAUDE.md, .cursor/rules/) for features only that editor supports.

Next.js 15 monorepo. TypeScript, Tailwind CSS, Drizzle ORM, pnpm workspaces.

## Workspace Structure

- `packages/web/` -- Next.js frontend (App Router, React 19)
- `packages/api/` -- Backend logic, Drizzle schema, database queries (see its own AGENTS.md)
- `packages/shared/` -- Shared TypeScript types and utilities

## Setup & Build

```sh
pnpm install
pnpm dev                # Start all packages
pnpm build              # Production build
pnpm typecheck          # TypeScript check (all workspaces)
pnpm test               # Run all tests
pnpm lint               # ESLint
```

All checks must pass before committing.

## Code Conventions

- Server Components by default. `"use client"` only at the smallest leaf needing hooks/events/browser APIs.
- Named exports. No default exports.
- `@/` path aliases for imports.
- Validate external input with Zod.
- Tailwind utility classes. `cn()` for conditional classes.

## Commit Guidelines

- One logical change per commit.
- Run `pnpm typecheck && pnpm lint && pnpm test` before committing.

## Don't

- Don't use the Pages Router, axios, `any`, or `@ts-ignore`.
- Don't import `drizzle-orm` from `packages/web/`. All DB access through `packages/api/`.
- Don't commit `.env` files.
