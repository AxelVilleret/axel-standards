---
paths:
  - "**/*.php"
---

# Clean code & style

Readable, intention-revealing PHP.

- Validate preconditions at the input.
- Fail fast; throw a clear exception.
- Use early returns; never use `else`.
- Avoid comments; let names explain.
- Natural comparisons, not Yoda (`$var === null`).
- `use` imports at top, alphabetical.
- Alias ambiguous class names.
- No unused imports.
- No global helpers; inject instead.
