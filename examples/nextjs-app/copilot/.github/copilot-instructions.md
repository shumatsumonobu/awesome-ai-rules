# Copilot Instructions

This is the main repo-level instruction file. Copilot reads it on every request. For file-type-specific rules, see `.github/instructions/` -- those files use `applyTo` frontmatter to activate only on matching files.

Next.js 15 monorepo with TypeScript, Tailwind CSS, and Drizzle ORM. pnpm workspaces with `packages/web` (frontend), `packages/api` (backend logic), and `packages/shared` (types).

## Code Style

- TypeScript strict mode. No `any`, no `@ts-ignore`.
- Named exports only. No default exports.
- Use `type` for props, not `interface`.
- Use `@/` path aliases.

## Database

- All database access goes through `packages/api/`. The web package never imports `drizzle-orm`.
- Use Drizzle's query builder, not raw SQL.
- Services return plain objects, not ORM row types.

## Don't

- Don't use the Pages Router, axios, `React.FC`, or barrel files.
- Don't commit `.env` files.
- Don't comment on code style or formatting issues.
