# Skills

Claude Code skills for product craft and the work of building considered software.

## Philosophy

These skills encode opinionated ways of working — not generic checklists, but specific processes for specific failure modes. Each one exists because the default approach (improvise each time, rely on memory, pattern-match without vocabulary) produces inconsistent results.

A skill earns its place when:

- The task recurs and benefits from structure
- The structure can be written down without killing judgment
- Running the skill produces better output than running without it

## Installation

```bash
npx skills@latest add kristianc/skills
```

Or add individual skills manually by copying the skill directory into your project's `.claude/skills/` folder.

## Skills

### Product

Skills for raising the craft level of what already exists.

| Skill | Description |
|-------|-------------|
| [improve-product-feel](./skills/product/improve-product-feel/SKILL.md) | Surface friction and propose polish opportunities. Use when you want to improve how a product feels, find defaults masquerading as design, or raise the quality of existing touchpoints. |

## Structure

```
skills/
├── .claude-plugin/
│   └── plugin.json
├── CLAUDE.md
├── README.md
└── skills/
    └── product/
        └── improve-product-feel/
            ├── SKILL.md
            ├── LANGUAGE.md
            ├── INTERACTION-DESIGN.md
            ├── CONTEXT-FORMAT.md
            └── ADR-FORMAT.md
```

## Creating new skills

See `CLAUDE.md` for the format requirements. Each skill needs:

1. A directory under `skills/<bucket>/`
2. A `SKILL.md` with `name` and `description` frontmatter
3. An entry in `.claude-plugin/plugin.json`
4. A reference in this README

## License

MIT
