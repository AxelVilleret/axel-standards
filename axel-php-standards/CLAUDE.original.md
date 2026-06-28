# CLAUDE.md — Global Preferences

## Defensive Programming in PHP

I follow the principles of modern defensive PHP programming, Clean Code, and strict OOP. Apply them with judgment, not as a rigid checklist: the goal is to reject invalid states by construction, improve readability, and ensure maintainability. A business rule encoded in a type is far cheaper than a scattered check or a production bug.

---

## General Principles & Clean Code

**Fail fast, fail loud & Early Returns** — Validate preconditions as close to the input as possible, throw a clear exception rather than hiding the problem. Never use `else`; you can always write it differently. Use early returns to improve readability, reduce cognitive load, and avoid deep nesting.

**Encode the contract in the types** — `declare(strict_types=1)`, type parameters / returns / properties. Rely on true native PHP typing first. Use PHPDoc strictly to complement native types (e.g., generics, array shapes), never to duplicate them. Avoid ambiguous returns (`false|null|int`). Use Enums over magic strings.

**Declarative & Expressive Syntax (`match` / array functions)** — Prefer array functions (`array_map`, `array_filter`, `array_reduce`) over `foreach` loops. Systematically use `match` expressions instead of `switch` or complex `if/elseif` chains. `match` is strictly typed, exhaustive, and returns a value, making your code far more readable and maintainable than imperative state mutations.

**Immutability by default** — Maximize the use of `readonly` to prevent mutability. Use `private(set)` for controlled state (PHP 8.4+), and the `with*()` pattern to "modify" objects. Ensure `with*()` methods are idempotent. Prefer `\DateTimeImmutable` over `\DateTime`.

**Encapsulation & SOLID** — Apply all SOLID principles rigorously. Favor composition over inheritance: build behavior by composing small, single-purpose objects injected via the constructor rather than creating deep, rigid class hierarchies. Whether injecting services in Symfony or isolating domain logic within PrestaShop modules, rely on interfaces and dependency injection instead of extending core classes. `final` must be used systematically (only abstractions should be inherited, unless a concrete inheritance is explicitly justified). Follow tell-don't-ask and the Law of Demeter. Extract complex logic into private sub-methods to enforce the Single Responsibility Principle (SRP).

**Value Objects against primitive obsession** — Use a self-validating domain type (`Email`, `Money`, `UserId`…) wherever a constraint is easy to forget. If the object exists, it is valid. (e.g., `Money` as `int` in cents, not `float`).

**Namespaces & Imports** — Always declare `use` statements at the top of the file for any external class or namespace. If a class name is ambiguous or unsuited for the current context, use an alias (`use SomeClass as DomainSomeClass`).

**Self-Documenting Code** — Avoid comments. Prefer explicit, intentional, and domain-driven naming for variables, methods, and classes to explain the *why* and *what*.

**Custom & Typed Domain Exceptions (Semantic Richness)** — Create custom business exceptions that carry high semantic meaning. An exception's name should immediately explain the exact business rule that was violated (e.g., `CartAlreadyCheckedOutException` instead of a generic `InvalidStateException`). Never throw a bare `\Exception` nor use a catch-all `catch (\Throwable)`. Catch only what you can handle; re-throw with context (`previous:`).

---

## Anti-patterns to Avoid

The `else` keyword, `switch` statements, `foreach` loops when an array function exists, arrays masquerading as objects, God objects, ambiguous boolean returns (`save(true)`), commented-out dead code, deep methods (>3 levels → extract to private sub-functions), and inheritance of convenience.

**AI-specific tics:** Avoid Yoda conditions (`null === $var`) unless explicitly forced by a linter; write natural comparisons (`$var === null`). Avoid unused `use` statements; sort them alphabetically. Avoid global helper functions; favor dependency injection.

---

## Apply with Judgment (Legacy & Architecture)

**New, isolated code** (services, jobs, value objects, DTOs) → Apply broadly: `strict_types`, full typing, `readonly`, VOs, declarative arrays, PHPStan at the highest sustainable level.

**Code integrating with a framework or CMS** → Follow the framework's conventions first. Don't force `readonly`/VOs/`final` where it breaks the framework's contracts or DI mechanisms. Keep the spirit without forcing the letter.

**Legacy code & The Boy Scout Rule** → Apply modern principles without taking unnecessary risks — do not break anything. Never blindly rewrite legacy or vendor code "to make it defensive". However, when modifying a method, leave it cleaner than you found it (e.g., add missing native types, extract ultra-complex blocks into private methods). Do not touch the rest of the file outside your task's scope.

**Modular Extensibility** → On modular architectures, never modify the core. Enforce strict isolation using hooks, event listeners, or service decorators (composition) rather than class overrides.

> Account for the project's actual PHP version before using newer features (property hooks, asymmetric visibility, etc.).

---

## Git Conventions

- Write concise commit messages in English.
- Use conventional commit prefixes (`feat:`, `fix:`, `chore:`, `refactor:`, etc.).
- **Mandatory Prefix:** Always prefix the message with `[<project_name>-<ticket_number>]`. You must extract this ticket identifier from the current branch name if it is present.

```
feat: [proshop-402] implement strict types for cart calculations
```

---

## Validation & Tooling

Before declaring a task as done, you must ensure the code compiles without warnings and passes the project's static analysis tools (`phpstan` / `psalm`) and linters (`php-cs-fixer` / `pint`).

Run the formatting/fix commands yourself using your terminal tools before delivering any code block.
