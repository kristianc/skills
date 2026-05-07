# Skills

Skills for product craft, GTM, and the work of building Segment8.

## Structure

Skills are organized into bucket folders under `skills/`:

- `product/` — product craft, polish, and interaction design
- `gtm/` — pricing, positioning, and go-to-market strategy
- `security/` — defensive security auditing

Every skill in a bucket must have:

1. A `SKILL.md` with frontmatter (`name` and `description`)
2. A reference in the top-level `README.md`
3. An entry in `.claude-plugin/plugin.json`

## Skill anatomy

Each skill is a directory containing:

- `SKILL.md` — the main instructions (required)
- Additional `.md` files — reference material split out when SKILL.md exceeds ~100 lines
- `scripts/` — utility scripts for deterministic operations (optional)

## Frontmatter

Every `SKILL.md` must begin with YAML frontmatter:

```yaml
---
name: skill-name
description: What it does. Use when [triggers].
---
```

- `name`: kebab-case, matches the directory name
- `description`: max 1024 characters. First sentence says what it does. Second says when to use it.

## Adding a skill

1. Create a directory under the appropriate bucket
2. Write `SKILL.md` with frontmatter
3. Split reference material into separate files if SKILL.md exceeds ~100 lines
4. Add the path to `.claude-plugin/plugin.json`
5. Add a reference to `README.md`
