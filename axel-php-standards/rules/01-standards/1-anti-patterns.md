---
paths:
  - "**/*.php"
---

# Anti-patterns to avoid

Constructs to reject on sight.

- No `else`, no `switch`.
- No `foreach` when an array fn fits.
- No arrays masquerading as objects.
- No God objects.
- No ambiguous boolean returns (`save(true)`).
- No commented-out dead code.
- No methods past three nesting levels.
- No inheritance of convenience.
