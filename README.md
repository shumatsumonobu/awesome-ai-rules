# Awesome AI Rules [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

AI coding editors like Cursor, Claude Code, and GitHub Copilot can read a rules file from your project -- things like "use Drizzle, not raw SQL" or "run tests before committing."

Each editor uses a different file for this: Cursor reads `.cursorrules`, Claude Code reads `CLAUDE.md`, Copilot reads `.github/copilot-instructions.md`, and so on. The best examples are scattered across thousands of repos.

**This list brings them together:**

- Compare all 7 formats side by side
- Learn from 40+ real-world examples (Next.js, Supabase, Sentry, etc.)
- Find tools that auto-generate rules for your project
- Decide which format fits your team

## Contents

- [Formats at a Glance](#formats-at-a-glance)
- [How to Choose](#how-to-choose)
- [Format Showcase](#format-showcase)
- [Tools](#tools)
- [CLAUDE.md](#claudemd)
- [.cursorrules](#cursorrules)
- [copilot-instructions.md](#copilot-instructionsmd)
- [AGENTS.md](#agentsmd)
- [GEMINI.md](#geminimd)
- [.windsurfrules](#windsurfrules)
- [.clinerules](#clinerules)
- [Multi-Format Repos](#multi-format-repos)
- [Writing Tips](#writing-tips)

## Formats at a Glance

| Format | Editor | File | Scope Control | Adoption |
|--------|--------|------|---------------|----------|
| [CLAUDE.md](#claudemd) | Claude Code | `CLAUDE.md` | Directory-level hierarchy | 37,000+ repos |
| [.cursorrules](#cursorrules) | Cursor | `.cursorrules` / `.cursor/rules/*.mdc` | Always / Auto / Agent / Manual (new `.mdc` format) | 40,000+ repos |
| [copilot-instructions.md](#copilot-instructionsmd) | GitHub Copilot | `.github/copilot-instructions.md` | Per-repo, per-org, `applyTo` patterns | Growing (Microsoft/.NET lead) |
| [AGENTS.md](#agentsmd) | Codex, Copilot, Gemini | `AGENTS.md` | Directory-level hierarchy | 60,000+ repos |
| [GEMINI.md](#geminimd) | Gemini CLI | `GEMINI.md` | Project-level | Google ecosystem only |
| [.windsurfrules](#windsurfrules) | Windsurf | `.windsurfrules` | Project-level | Early stage |
| [.clinerules](#clinerules) | Cline | `.clinerules` | Project-level | Community-driven |

## How to Choose

| Situation | Best Format | Why |
|-----------|------------|-----|
| Team uses Claude Code | CLAUDE.md | Directory hierarchy, local overrides, `.claude/` directory |
| Team uses Cursor | `.cursor/rules/*.mdc` | 4 scope modes give fine-grained control per file type |
| Team uses Codex, Copilot, or Gemini | AGENTS.md | One file works across all three simultaneously |
| Monorepo with diverse packages | CLAUDE.md or AGENTS.md | Directory hierarchy handles package-specific rules naturally |
| Want per-filetype rules | Cursor `.mdc` or Copilot `.instructions.md` | Glob / `applyTo` patterns scope rules to specific file types |
| Want maximum editor coverage | AGENTS.md + editor-specific file | Shared baseline + editor-specific features |

## Format Showcase

The same project needs different rule *structures* per editor, not just different *formats*. The [`examples/nextjs-app/`](examples/nextjs-app/) folder shows one monorepo configured for 4 editors:

**Claude Code** -- directory-level hierarchy
- [Root CLAUDE.md](examples/nextjs-app/claude/CLAUDE.md) -- project-wide rules
- [web/CLAUDE.md](examples/nextjs-app/claude/packages/web/CLAUDE.md) -- subdirectory override
- [api/CLAUDE.md](examples/nextjs-app/claude/packages/api/CLAUDE.md) -- subdirectory override

**Cursor** -- `.mdc` files with 4 scope modes
- [always.mdc](examples/nextjs-app/cursor/.cursor/rules/always.mdc) -- Always
- [react.mdc](examples/nextjs-app/cursor/.cursor/rules/react.mdc) -- Auto Attached (by glob)
- [testing.mdc](examples/nextjs-app/cursor/.cursor/rules/testing.mdc) -- Agent Requested
- [database.mdc](examples/nextjs-app/cursor/.cursor/rules/database.mdc) -- Manual

**GitHub Copilot** -- topic-based `.instructions.md` with `applyTo` patterns
- [copilot-instructions.md](examples/nextjs-app/copilot/.github/copilot-instructions.md) -- repo-level
- [react.instructions.md](examples/nextjs-app/copilot/.github/instructions/react.instructions.md) -- `applyTo: **/*.tsx`
- [testing.instructions.md](examples/nextjs-app/copilot/.github/instructions/testing.instructions.md) -- `applyTo: **/*.test.*`

**AGENTS.md** -- cross-editor hierarchy (Codex + Copilot + Gemini)
- [Root AGENTS.md](examples/nextjs-app/agents/AGENTS.md) -- shared baseline
- [api/AGENTS.md](examples/nextjs-app/agents/packages/api/AGENTS.md) -- subdirectory extension

## Tools

These tools scan your codebase and generate project-specific rule files, so you don't have to start from scratch:

| Tool | What It Does | Formats |
|------|-------------|---------|
| [caliber](https://github.com/caliber-ai-org/ai-setup) (1k stars) | `npx @rely-ai/caliber bootstrap` -- scans your repo and generates rules for multiple editors at once | CLAUDE.md, .cursorrules, AGENTS.md, copilot |
| [block/ai-rules](https://github.com/block/ai-rules) | Block (Square) multi-editor rule manager. Define once, generate for 11 editors | CLAUDE.md, AGENTS.md, and 9 more |
| [rule-porter](https://github.com/nedcodes-ok/rule-porter) | Convert existing rules between formats (bidirectional CLI) | CLAUDE.md, .cursorrules, AGENTS.md, copilot |
| [ai-rulez](https://github.com/Goldziher/ai-rulez) | `npx ai-rulez@latest generate` -- write rules once in `.ai-rulez/`, generate for 20+ editors | CLAUDE.md, .cursorrules, AGENTS.md, copilot, and 16 more |
| [AgentRuleGen](https://www.agentrulegen.com/) | Web UI -- pick your stack and editor, get generated rules | All major formats |

## CLAUDE.md

Used by [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

- Place `CLAUDE.md` at the repo root for project-wide rules
- Add `CLAUDE.md` in subdirectories for scoped overrides (directory hierarchy)
- `CLAUDE.local.md` for personal overrides (gitignored)

### Notable Examples

| Repository | Stars | Language / Framework | What Makes It Good |
|------------|-------|---------------------|-------------------|
| [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) | 189k | Multi | Karpathy's coding principles distilled into a CLAUDE.md (not by Karpathy himself) |
| [vercel/next.js](https://github.com/vercel/next.js) | 140k | JavaScript / TypeScript | 25KB. Bug reproduction workflow, legacy zone warnings, monorepo navigation guide |
| [supabase/supabase](https://github.com/supabase/supabase) | 106k | TypeScript | Uses `.claude/CLAUDE.md`. Language-based routing with PR checklist |
| [getsentry/sentry](https://github.com/getsentry/sentry) | 44k | Python / TypeScript | Minimal pointer (11 bytes) -- delegates to other docs. Great "less is more" example |
| [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) | 37k | Python | Concise monorepo package listing with build instructions |
| [lerna/lerna](https://github.com/lerna/lerna) | 36k | TypeScript | Package structure, testing strategy, and release procedures |
| [getzep/graphiti](https://github.com/getzep/graphiti) | 29k | Python | Type checking commands, uv/pytest setup, knowledge-graph-specific constraints |
| [openai/openai-agents-python](https://github.com/openai/openai-agents-python) | 28k | Python | OpenAI's own multi-agent framework. Detailed project structure and testing guide |
| [anthropics/claude-code-action](https://github.com/anthropics/claude-code-action) | 8k | TypeScript | Anthropic's own GitHub Action -- a textbook example of CLAUDE.md structure |
| [gaearon/overreacted.io](https://github.com/gaearon/overreacted.io) | 7k | Next.js / MDX | Dan Abramov's blog. Personality + technical depth coexist |
| [coder/claudecode.nvim](https://github.com/coder/claudecode.nvim) | 3k | Lua | 23KB -- one of the largest. Exhaustive development conventions for a Neovim extension |

**See also:** [josix/awesome-claude-md](https://github.com/josix/awesome-claude-md) -- a dedicated collection of 100+ CLAUDE.md examples.

## .cursorrules

Used by [Cursor](https://cursor.com).

- Legacy: single `.cursorrules` file (applies to all requests)
- New: `.cursor/rules/*.mdc` with YAML frontmatter and four scope modes: `Always`, `Auto Attached` (by glob), `Agent Requested`, `Manual`

### Notable Examples

| Repository | Stars | Language / Framework | What Makes It Good |
|------------|-------|---------------------|-------------------|
| [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) | 78k | Multi-language | "Lazy senior developer" philosophy -- prevents AI from over-engineering. Supports `.cursor/rules/` and 16 other harnesses |
| [PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) | 40k | Multi (214+ rules) | The definitive collection. Categorized by framework, language, testing, deployment, and security |
| [wshobson/agents](https://github.com/wshobson/agents) | 38k | Multi-harness | 92 plugins, 199 agents. Markdown-first, auto-generates `.cursor/rules/` for 5 platforms |
| [grapeot/devin.cursorrules](https://github.com/grapeot/devin.cursorrules) | 6k | Python | Turns Cursor into "90% of Devin". Self-evolving via lessons-learned accumulation |
| [sanjeed5/awesome-cursor-rules-mdc](https://github.com/sanjeed5/awesome-cursor-rules-mdc) | 4k | Multi | Auto-generates per-library `.mdc` files using LLM + search |
| [flyeric0212/cursor-rules](https://github.com/flyeric0212/cursor-rules) | 2k | 20+ languages | 4-layer `.mdc` structure: base > language > framework > tool |
| [ciembor/agent-rules-books](https://github.com/ciembor/agent-rules-books) | 2k | Multi | 14 classic books (Clean Code, DDD, etc.) distilled into agent rules. Mini/nano/full tiers |
| [matank001/cursor-security-rules](https://github.com/matank001/cursor-security-rules) | 373 | Security-focused | Secure coding, secrets management, dependency hygiene |

**See also:** [cursor.directory](https://cursor.directory) -- a community-driven search site for Cursor rules.

## copilot-instructions.md

Used by [GitHub Copilot](https://docs.github.com/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot).

- `.github/copilot-instructions.md` for repo-level instructions
- `.github/instructions/*.instructions.md` for topic-specific rules (with `applyTo` file patterns)

### Notable Examples

| Repository | Stars | Language / Framework | What Makes It Good |
|------------|-------|---------------------|-------------------|
| [home-assistant/core](https://github.com/home-assistant/core) | 88k | Python | Code review standards (no style nitpicks), Python 3.14 compliance, "comments explain why only" |
| [block/goose](https://github.com/block/goose) | 51k | Rust | "Comment only with 80%+ confidence" -- pioneered the confidence-threshold pattern |
| [github/awesome-copilot](https://github.com/github/awesome-copilot) | 36k | Multi | Official GitHub collection with 189+ instruction examples across languages and frameworks |
| [dotnet/maui](https://github.com/dotnet/maui) | 23k | C# / .NET MAUI | Defines custom agents (PR, test, sandbox) within the instructions file |
| [dotnet/runtime](https://github.com/dotnet/runtime) | 18k | C# | Component-specific build instructions, AI-generated content disclosure requirements |
| [dotnet/AspNetCore.Docs](https://github.com/dotnet/AspNetCore.Docs) | 13k | C# / ASP.NET | Documentation conventions, .NET version reference rules, file naming (lowercase + hyphens) |
| [Hack23/cia](https://github.com/Hack23/cia) | 231 | Java | Links ISMS security policy -- applies governance to AI-generated code |

**See also:** [awesome-copilot.github.com/instructions](https://awesome-copilot.github.com/instructions/) -- GitHub's official searchable instruction catalog.

## AGENTS.md

An emerging cross-editor standard stewarded by the [Agentic AI Foundation](https://agenticaifoundation.org) (Linux Foundation).

- Supported by OpenAI Codex, GitHub Copilot, and Gemini
- Directory-level hierarchy (like CLAUDE.md) -- place `AGENTS.md` at any level of your project tree

### Notable Examples

| Repository | Stars | Language / Framework | What Makes It Good |
|------------|-------|---------------------|-------------------|
| [openai/codex](https://github.com/openai/codex) | 91k | Rust / TypeScript | Hierarchical AGENTS.md files -- the reference implementation for directory-level agent instructions |
| [apache/airflow](https://github.com/apache/airflow) | 46k | Python | Workflow orchestration. Build/test procedures and code conventions |
| [openai/openai-agents-python](https://github.com/openai/openai-agents-python) | 28k | Python | Multi-agent framework. Repo structure, testing, and commit guidelines |
| [jlowin/fastmcp](https://github.com/jlowin/fastmcp) | 26k | Python | MCP server/client framework. Explicit behavioral constraints (e.g., "never publish releases") |
| [vercel/ai](https://github.com/vercel/ai) | 25k | TypeScript / Next.js | Vercel's AI SDK. Agent-oriented development guide |
| [coleam00/Archon](https://github.com/coleam00/Archon) | 23k | Python | AI coding agent workflow engine. YAML-based procedure definitions |
| [mark3labs/mcp-go](https://github.com/mark3labs/mcp-go) | 9k | Go | Go MCP implementation. External data source integration guide |

**See also:** [GitHub Blog: How to write a great AGENTS.md](https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories/) -- analysis of 2,500+ repositories.

## GEMINI.md

Used by [Gemini CLI](https://github.com/google-gemini/gemini-cli). Currently adopted almost exclusively within Google's ecosystem.

- `GEMINI.md` for project-level instructions
- `.gemini/styleguide.md` for style-specific rules

### Notable Examples

| Repository | Stars | Language / Framework | What Makes It Good |
|------------|-------|---------------------|-------------------|
| [google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli) | 106k | TypeScript | Gemini CLI itself. Architecture, testing, and contribution conventions |
| [GoogleCloudPlatform/generative-ai](https://github.com/GoogleCloudPlatform/generative-ai) | 16k | Python / Jupyter | GCP generative AI samples. Uses `.gemini/styleguide.md` for style guide |
| [GoogleCloudPlatform/agent-starter-pack](https://github.com/GoogleCloudPlatform/agent-starter-pack) | 5k | Python | GCP agent templates. CI/CD, evaluation, and observability best practices |
| [GoogleCloudPlatform/cloud-foundation-fabric](https://github.com/GoogleCloudPlatform/cloud-foundation-fabric) | 2k | Terraform (HCL) | GCP landing zone toolkit. Terraform module structure conventions |

## .windsurfrules

Originally used by Windsurf, built by Codeium. Codeium was [acquired by Cognition (Devin)](https://devin.ai) in 2025. The format's future is uncertain post-acquisition, but existing rule files still work.

### Notable Examples

| Repository | Stars | Language / Framework | What Makes It Good |
|------------|-------|---------------------|-------------------|
| [CoderGamester/mcp-unity](https://github.com/CoderGamester/mcp-unity) | 2k | C# / Unity | Unity Editor MCP plugin. Defines AI constraints for Unity operations |
| [kinopeee/windsurfrules](https://github.com/kinopeee/windsurfrules) | -- | Multi (templates) | Cursorrules adapted for Windsurf Cascade. Good starting point |

## .clinerules

Used by [Cline](https://github.com/cline/cline) (VS Code extension). Supports both a single `.clinerules` file and a `.clinerules/` directory for multiple rule files.

### Notable Examples

| Repository | Stars | Language / Framework | What Makes It Good |
|------------|-------|---------------------|-------------------|
| [cline/cline](https://github.com/cline/cline) | 58k | TypeScript | Cline itself. Uses `.clinerules/` directory |
| [gitpod-io/gitpod](https://github.com/gitpod-io/gitpod) | 14k | TypeScript / Go | Cloud dev environments. Memory Bank pattern for cross-session context persistence |
| [pdfme/pdfme](https://github.com/pdfme/pdfme) | 3k | TypeScript / React | PDF generation library. WYSIWYG template designer development conventions |

**See also:** [cline/clinerules](https://github.com/cline/clinerules) -- official community collection of Cline rules, skills, and workflows.

## Multi-Format Repos

These repositories maintain rule files for multiple AI editors -- useful for seeing how the same project adapts its instructions across tools.

| Repository | Stars | Formats Used |
|------------|-------|-------------|
| [vercel/next.js](https://github.com/vercel/next.js) | 140k | CLAUDE.md, AGENTS.md |
| [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) | 78k | `.cursor/rules/`, CLAUDE.md, AGENTS.md, copilot-instructions, and 13 more |
| [wshobson/agents](https://github.com/wshobson/agents) | 38k | `.cursor/rules/`, CLAUDE.md, AGENTS.md, copilot-instructions, .windsurfrules |
| [openai/openai-agents-python](https://github.com/openai/openai-agents-python) | 28k | CLAUDE.md, AGENTS.md |
| [caliber-ai-org/ai-setup](https://github.com/caliber-ai-org/ai-setup) | 1k | Auto-generates CLAUDE.md, `.cursor/rules/`, AGENTS.md via `npx @rely-ai/caliber` |

## Writing Tips

Patterns that work across all formats, distilled from the best examples above.

**Structure your file like this:**
1. Project overview (what this repo is, 1-2 sentences)
2. Tech stack (language, framework, key libraries)
3. Repository structure (directory roles)
4. Build & test commands (copy-pasteable)
5. Coding conventions (only project-specific ones -- skip universal best practices)
6. Do and Don't (explicit boundaries)

**What works:**

| Pattern | Good | Bad |
|---------|------|-----|
| Short imperatives | "Use named exports" | "It is generally recommended to follow good export practices" |
| Explain *why* | "Use date-fns (moment.js is deprecated, bloats bundle)" | "Use date-fns" |
| Copy-pasteable commands | `npm run test -- --watch` | "run the tests" |
| Explicit boundaries | "Don't comment on lint/format issues" | (nothing -- AI guesses) |

Keep it under 1,000 lines -- quality degrades beyond that.

**What doesn't work:**
- Restating universal best practices ("write clean code", "follow SOLID")
- Duplicating what's in your README, CONTRIBUTING.md, or linter config
- Making it too long -- AI models lose signal in noise just like humans

## Contributing

Contributions welcome! Please read the [Contributing Guide](CONTRIBUTING.md) before submitting a PR.

To add a new example:
1. Verify the repository actually has the rule file (link to the specific file, not just the repo)
2. Include the approximate star count
3. Write a one-line description of what makes it a good example
4. Place it in the correct format section, ordered by star count

## License

[MIT](LICENSE)
