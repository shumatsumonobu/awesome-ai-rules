# packages/web

Next.js 15 frontend. This file extends the root CLAUDE.md -- Claude Code reads the root first, then merges this file when working inside `packages/web/`.

## What This File Adds (not in root)

- Use `@web/` path alias (not `@/`) for imports within this package.
- Use `loading.tsx` per route for streaming, `error.tsx` for error boundaries with retry.
- Use Server Actions for mutations. Define in `actions.ts` with `"use server"`.
- Use `cn()` (clsx + tailwind-merge) for conditional Tailwind classes.

## What This File Overrides (different from root)

- **Commands**: Use `pnpm --filter web dev/build/test` here, not the root-level `pnpm dev`.
- **Testing**: Use React Testing Library + `screen.getByRole()` for component tests. Root says "Vitest" but doesn't specify the DOM testing approach.

## Boundary Rule

Route handlers (`app/api/`) are for webhooks only. All business logic lives in `packages/api/`. If you're asked to "add an API endpoint," create a service in `packages/api/` and call it from a Server Action here.

## Directory Structure

```
src/
  app/              # App Router (file-based routing)
  components/ui/    # Reusable primitives (Button, Card)
  components/features/  # Feature-specific composites
  lib/              # Utilities, API clients
  hooks/            # Custom React hooks
```
