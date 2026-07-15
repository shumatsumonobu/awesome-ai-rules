---
applyTo: "**/*.tsx"
---

# React Instructions

This file activates ONLY when Copilot is working on `.tsx` files (controlled by the `applyTo` pattern above). The main `copilot-instructions.md` applies globally; this adds React-specific rules on top.

- Components are Server Components by default. Add `"use client"` only when the component uses hooks, event handlers, or browser APIs.
- Keep `"use client"` at the smallest leaf. Never mark an entire page or layout as client.
- Define props as `type ButtonProps = { ... }` in the same file.
- Use `cn()` from `@/lib/utils` for conditional Tailwind classes.
- Use `forwardRef` for components wrapping native HTML elements.
- Use `loading.tsx` for route-level streaming. Use `error.tsx` for error boundaries with a retry button.
- Fetch data in Server Components with `async/await`. Server Actions for mutations.
- Never use `useEffect` for data fetching.
