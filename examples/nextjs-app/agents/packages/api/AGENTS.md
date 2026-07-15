# packages/api

Backend logic package. This file extends the root AGENTS.md with API-specific conventions. Like CLAUDE.md, AGENTS.md supports directory hierarchy -- editors that read AGENTS.md (Codex, Copilot, Gemini) will merge this file with the root when working in this directory.

## Structure

```
src/
  schema/       # Drizzle table definitions (one file per table)
  services/     # Business logic layer
  queries/      # Reusable query builders
  migrations/   # Generated migration files (do not edit by hand)
```

## Commands

```sh
pnpm --filter api test              # Unit tests
pnpm --filter api db:generate       # Generate migration from schema changes
pnpm --filter api db:migrate        # Apply pending migrations
pnpm --filter api db:studio         # Open Drizzle Studio
```

## Conventions

- Use Drizzle's query builder. Raw SQL only when query builder cannot express the query.
- Services return plain objects, never Drizzle row types.
- Wrap multi-table writes in transactions.
- Throw domain-specific errors (`NotFoundError`, `ValidationError`), not raw database errors.
- No frontend code: never import from `react`, `next`, or any UI library here.
