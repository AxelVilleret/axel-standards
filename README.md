# axel-standards

Personal Claude Code marketplace for reusable coding-standards plugins. A plugin ships a set of scoped rules, and may also ship skills, agents, or commands. Every plugin follows the same AIDD rule structure, documented below.

## Plugins

| Plugin | Scope | What it enforces |
| --- | --- | --- |
| `axel-php-standards` | `**/*.php` | Clean code, SOLID, native typing, immutability, value objects, domain exceptions |

## Install

Register the marketplace once, then install per project:

```text
/plugin marketplace add axelvilleret/axel-standards
/plugin install axel-php-standards@axel-standards
```

Before publishing to GitHub, add it from the local path instead:

```text
/plugin marketplace add /home/axelvilleret/projects/axel-standards
/plugin install axel-php-standards@axel-standards
```

## Activation

A plugin's surfaces are wired up by two different mechanisms — and rules are the exception.

### Skills, agents, commands, hooks — via the plugin

Install at `user` scope (available everywhere), then enable a plugin only where it fits via each project's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "axel-php-standards@axel-standards": true
  }
}
```

This loads the plugin's skills, agents, commands, and hooks. It does **not** load its rules.

### Rules — symlink only

Enabling a plugin does not activate its rules. The only way to wire a plugin's rules into a project is to symlink its `rules/` directory into the project's `.claude/rules/`, where Claude Code discovers them:

```bash
mkdir -p .claude/rules
ln -s ~/projects/axel-standards/axel-php-standards/rules .claude/rules/axel-php
```

The rules then resolve at `.claude/rules/axel-php/<category>/<rule>.md` and apply according to each file's scope. Editing a rule in this repository updates every project that links it.

Notes:
- The symlink targets an absolute home path, so it only resolves on the machine where this repository lives at that location.
- Add the link (e.g. `.claude/rules/axel-php`) to the project's `.gitignore` — it points outside the repository and is a per-developer convenience, not shared state.

## How the rules are structured

Every plugin's `rules/` follows the AIDD rule convention so rules stay readable, scoped, and mergeable with the rules a project already has.

### Directory layout

```text
rules/
  <NN>-<category>/
    <#>-<slug>.md
```

`<NN>` is the zero-padded category index, `<#>` the same index without padding, and `<slug>` a short kebab-case topic name. Example: `rules/02-programming-languages/2-php.md`.

### Category taxonomy

Files are filed under one of ten fixed categories. Pick the most specific match: framework `03`, language syntax `02`, agnostic style `01`, architecture `00`, then `04`–`08` in order, else `09`.

| #    | Category                   | Covers                                          |
| ---- | -------------------------- | ----------------------------------------------- |
| `00` | `architecture`             | System patterns (Clean, Hexagonal, API design)  |
| `01` | `standards`                | Code style, naming, imports                     |
| `02` | `programming-languages`    | Language-specific rules                         |
| `03` | `frameworks-and-libraries` | Framework/library patterns                      |
| `04` | `tooling`                  | Tool and infra config                           |
| `05` | `testing`                  | Test patterns                                   |
| `06` | `design-patterns`          | Code design (Repository, Factory, …)            |
| `07` | `quality`                  | Performance and security                        |
| `08` | `domain`                   | Business logic (entities, DTOs)                 |
| `09` | `other`                    | Miscellaneous                                   |

### Scoping

Each rule declares the files it governs in YAML frontmatter. A scoped rule lists globs under `paths`; omit the frontmatter block entirely for an all-files rule.

```markdown
---
paths:
  - "**/*.php"
---

# PHP language conventions
```

### Authoring contract

- One file, one topic. Split a crowded rule into several files.
- Bullets only, no prose. One ultra-short rule per bullet (3–7 words); less is more.
- Group related bullets under a `## group` heading when the rule spans several themes; skip groups for a short rule.
- Add at most one tiny, correct example per group, and only when it removes ambiguity.
- Write rules in English regardless of the working language.

### Adding a rule

1. Choose the category and create `rules/<NN>-<category>/` if it does not exist.
2. Name the file `<#>-<slug>.md` matching the category index.
3. Add `paths` frontmatter for a scoped rule, or no frontmatter for an all-files rule.
4. Write the bullets following the authoring contract above.

## Add another plugin

Scaffold a sibling (e.g. `axel-react-standards/`) with its own `.claude-plugin/plugin.json` and `rules/`, then append an entry to `.claude-plugin/marketplace.json`. Its rules follow the same structure and activation as above.
