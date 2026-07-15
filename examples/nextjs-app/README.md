# Format Showcase: Next.js Monorepo

The same Next.js monorepo project, configured for 4 different AI editors. Each folder demonstrates that editor's **unique scoping features** -- not just the same rules reformatted.

## Concrete Scenario: "Add a database query"

Watch how each editor handles the same request differently based on its scoping model:

**You're editing `packages/api/src/services/userService.ts` and ask the AI to "add a query to find users by email."**

| Editor | What Happens | Why |
|--------|-------------|-----|
| **Claude Code** | Reads root `CLAUDE.md` (project-wide rules) + `packages/api/CLAUDE.md` (DB-specific rules like "use Drizzle query builder, not raw SQL"). The web/ CLAUDE.md is NOT loaded. | Directory hierarchy: files merge based on your current location |
| **Cursor** | Loads `always.mdc` (always on) + `database.mdc` (glob matches `packages/api/**/*.ts`). React rules from `react.mdc` are NOT loaded because the file isn't `.tsx`. | Glob-based activation: each .mdc file declares when it fires |
| **GitHub Copilot** | Loads `copilot-instructions.md` (global) + `react.instructions.md` only if it matches the applyTo pattern. No DB-specific instructions file exists yet. | Topic-based: you'd need to create a `database.instructions.md` with `applyTo: "packages/api/**"` |
| **AGENTS.md** | Reads root `AGENTS.md` + `packages/api/AGENTS.md`. Same hierarchy concept as Claude, but this file also works in Codex and Gemini. | Cross-editor hierarchy: one file, three editors |

**Now you switch to `packages/web/src/components/UserCard.tsx`:**

| Editor | What Changes |
|--------|-------------|
| **Claude Code** | `packages/api/CLAUDE.md` unloads, `packages/web/CLAUDE.md` loads instead. Now the AI knows about `@web/` path alias and React Testing Library conventions. |
| **Cursor** | `database.mdc` deactivates (glob no longer matches), `react.mdc` activates (matches `.tsx`). The AI now sees React component rules instead of Drizzle rules. |
| **GitHub Copilot** | `react.instructions.md` activates (applyTo matches `.tsx`). |
| **AGENTS.md** | `packages/api/AGENTS.md` unloads. No `packages/web/AGENTS.md` exists, so only root applies. |

## What Each Format Can Do

### Claude Code (`claude/`)

**Unique feature: directory-level inheritance with override**

```
CLAUDE.md                          # Root: monorepo-wide rules
packages/web/CLAUDE.md             # EXTENDS root: adds React conventions, OVERRIDES commands
packages/api/CLAUDE.md             # EXTENDS root: adds Drizzle conventions, blocks React imports
```

The subdirectory files explicitly call out what they ADD and what they OVERRIDE from the root. This is how Claude Code handles a monorepo where different packages have different conventions.

### Cursor (`cursor/`)

**Unique feature: `.mdc` files with 4 scope modes**

```
.cursor/rules/
  always.mdc       # alwaysApply: true -- 4 lines, just TypeScript strictness + commit rules
  react.mdc        # globs: ["packages/web/**/*.tsx"] -- ONLY loads for React files
  testing.mdc      # No globs -- agent reads description, pulls in when writing tests
  database.mdc     # globs: ["packages/api/**/*.ts"] -- ONLY loads for API files
```

The key insight: `always.mdc` is intentionally minimal (4 rules). Everything else is conditional -- React rules never contaminate database code and vice versa. `testing.mdc` has no globs at all; the agent reads its description and decides to pull it in based on the task.

### GitHub Copilot (`copilot/`)

**Unique feature: topic-based `.instructions.md` with `applyTo` patterns**

```
.github/
  copilot-instructions.md                 # Main repo-level instructions
  instructions/
    react.instructions.md                 # applyTo: "**/*.tsx"
    testing.instructions.md               # applyTo: "**/*.test.{ts,tsx}"
```

Similar to Cursor's globs but organized by topic rather than by scope mode. The `applyTo` frontmatter controls when each topic-specific file activates.

### AGENTS.md (`agents/`)

**Unique feature: one file, multiple editors**

```
AGENTS.md                          # Root: project-wide conventions
packages/api/AGENTS.md             # API-specific conventions
```

Same directory hierarchy as Claude, but the AGENTS.md spec is supported by OpenAI Codex, GitHub Copilot, and Gemini simultaneously. If your team uses different editors, AGENTS.md gives you one file that works everywhere.

## When to Choose What

| Situation | Best Format | Why |
|-----------|------------|-----|
| Team uses Claude Code | CLAUDE.md | Richest feature set for Claude (local overrides, `.claude/` directory) |
| Team uses Cursor | `.cursor/rules/*.mdc` | 4 scope modes give fine-grained control per file type |
| Team uses mixed editors | AGENTS.md | One file works in Codex, Copilot, and Gemini |
| Team uses GitHub Copilot | Both copilot-instructions.md AND AGENTS.md | Copilot reads both; use copilot-specific features in one, shared rules in the other |
| Monorepo with diverse packages | CLAUDE.md or AGENTS.md | Directory hierarchy handles package-specific rules naturally |
| Want per-filetype rules without monorepo | Cursor `.mdc` or Copilot `.instructions.md` | Glob/applyTo patterns work within a single package |
