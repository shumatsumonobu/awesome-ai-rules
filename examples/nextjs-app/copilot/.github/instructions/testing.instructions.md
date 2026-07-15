---
applyTo: "**/*.test.{ts,tsx}"
---

# Testing Instructions

This file activates ONLY when Copilot is working on test files (controlled by the `applyTo` pattern above). Unlike Cursor's agent-requested mode, Copilot requires an explicit file pattern -- it won't pull this in based on task context alone.

- Use Vitest with React Testing Library.
- Test user-visible behavior, not implementation details.
- Query by role (`getByRole`), text, or label. Avoid `getByTestId` unless no accessible query works.
- Use `@testing-library/user-event` for interactions, not `fireEvent`.
- Mock Server Actions and API calls. Never hit real endpoints in tests.
- Use factory functions for test data, not inline object literals.
- Co-locate tests: `Button.test.tsx` next to `Button.tsx`.
