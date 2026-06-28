# Archived global preferences

Your previous global `~/.claude/CLAUDE.md` (PHP defensive-programming preferences) after the global file was switched to the AIDD `AGENTS.md` template.

- `CLAUDE.original.md` — the previous global file, verbatim.
- `rules/` — the same content split into AIDD rule files by category (scoped to `**/*.php`, except git conventions which is all-files).

To reactivate any rule globally, move its file into `~/.claude/rules/<category>/`.

## Rule index

| Category | File | Topic |
| --- | --- | --- |
| 00-architecture | `0-solid-oop.md` | SOLID, composition, DI, `final`, SRP |
| 00-architecture | `0-modularity-and-legacy.md` | no core edits, hooks/decorators, Boy Scout |
| 01-standards | `1-clean-code.md` | early returns, no `else`, naming, imports |
| 01-standards | `1-anti-patterns.md` | constructs to reject |
| 02-programming-languages | `2-php.md` | strict typing, `match`, array fns, immutability |
| 04-tooling | `4-php-static-analysis.md` | PHPStan/Psalm, php-cs-fixer/Pint |
| 06-design-patterns | `6-value-objects.md` | VOs against primitive obsession |
| 06-design-patterns | `6-domain-exceptions.md` | semantic custom exceptions |
| 09-other | `9-git-conventions.md` | conventional commits, ticket prefix |
