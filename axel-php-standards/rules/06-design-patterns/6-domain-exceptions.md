---
paths:
  - "**/*.php"
---

# Domain exceptions

Exceptions that name the violated rule.

- Throw custom, semantically named exceptions.
- Name the violated business rule.
- Never throw bare `\Exception`.
- No catch-all `catch (\Throwable)`.
- Catch only what you handle.
- Re-throw with `previous:` context.
