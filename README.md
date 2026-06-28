# axel-standards

Personal Claude Code marketplace for reusable coding-standards plugins. Each plugin ships a set of scoped rules; you enable the ones a project needs.

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

## Per-project activation

Install at `user` scope (available everywhere), then enable only where it fits via each project's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "axel-php-standards@axel-standards": true
  }
}
```

## Add another plugin

Scaffold a sibling (e.g. `axel-react-standards/`) with its own `.claude-plugin/plugin.json` and `rules/`, then append an entry to `.claude-plugin/marketplace.json`.
