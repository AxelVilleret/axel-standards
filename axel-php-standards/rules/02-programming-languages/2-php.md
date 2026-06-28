---
paths:
  - "**/*.php"
---

# PHP language conventions

Encode the contract in native types; stay declarative and immutable.

## Typing

- `declare(strict_types=1)` in every file.
- Type params, returns, properties.
- PHPDoc complements native types only.
- Avoid ambiguous returns (`false|null|int`).
- Enums over magic strings.
- Check the project's PHP version first.

## Expressive syntax

- `match` over `switch` / `if-elseif`.
- Array functions over `foreach`.

## Immutability

- Prefer `readonly` properties.
- `private(set)` for controlled state (8.4+).
- `with*()` methods, idempotent.
- `\DateTimeImmutable` over `\DateTime`.
