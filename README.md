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

## Usage

Once installed, invoke a skill by name in Claude Code:

```
/improve-product-feel
```

The skill takes over from there — it reads your codebase, walks real flows, and guides you through the process. No configuration needed.

For pricing research, point it at a product or market:

```
/pricing-research
```

It will ask you for the inputs it needs (segments, value drivers, constraints) and produce a structured report.

## Skills

### Product

Skills for raising the craft level of what already exists.

| Skill | Description |
|-------|-------------|
| [improve-product-feel](./skills/product/improve-product-feel/SKILL.md) | Surface friction and propose polish opportunities. Find defaults masquerading as design and raise the quality of existing touchpoints. |
| [audit-empty-states](./skills/product/audit-empty-states/SKILL.md) | Find every empty state and rewrite them from "nothing here" into actionable moments. |
| [error-copy-review](./skills/product/error-copy-review/SKILL.md) | Find every error message and rewrite it in the product's voice with a path to recovery. |
| [onboarding-friction](./skills/product/onboarding-friction/SKILL.md) | Walk the first-run experience and map every moment the product assumes knowledge the user doesn't have. |
| [ai-design-polish](./skills/product/ai-design-polish/SKILL.md) | Elevate AI-generated designs from functional-but-generic to premium. Close the gap between "generated" and "crafted" for landing pages, dashboards, and marketing sites. |
| [trust-building-patterns](./skills/product/trust-building-patterns/SKILL.md) | Audit and improve trust signals — social proof, pricing transparency, security indicators, honest copy, and risk reduction patterns that make users confident enough to sign up, pay, and stay. |
| [website-teardown](./skills/product/website-teardown/SKILL.md) | Perform a structured teardown of any website, breaking down what works, what doesn't, and why across first impression, copy, design, conversion, and trust. |

### GTM

Pricing, positioning, and go-to-market strategy.

| Skill | Description |
|-------|-------------|
| [pricing-research](./skills/gtm/pricing-research/SKILL.md) | Produce a pricing analysis grounded in segmentation, targeting and positioning, with banded recommendations, Van Westendorp sensitivity analysis, and optimal revenue mix. |
| [competitive-positioning](./skills/gtm/competitive-positioning/SKILL.md) | Analyze what competitors claim vs. deliver and define where the product wins on axes they can't match. |
| [ideal-customer-profile](./skills/gtm/ideal-customer-profile/SKILL.md) | Define who gets disproportionate value from the product with qualifying and disqualifying criteria. |
| [launch-brief](./skills/gtm/launch-brief/SKILL.md) | Structure a launch around audience, message, channel, and timing through a grilling process that forces articulation of why anyone should care. |
| [seo-aeo](./skills/gtm/seo-aeo/SKILL.md) | Audit and improve search engine optimization and answer engine optimization — traditional SEO for Google rankings plus AEO for AI-powered search (ChatGPT, Perplexity, AI Overviews). |

### Security

Defensive security auditing for your own product.

| Skill | Description |
|-------|-------------|
| [security-review](./skills/security/security-review/SKILL.md) | Audit a codebase for security vulnerabilities — OWASP Top 10, auth/authz gaps, secrets in code, injection vectors, and dependency risks. |

### Engineering

Production engineering and operational readiness.

| Skill | Description |
|-------|-------------|
| [production-ready](./skills/engineering/production-ready/SKILL.md) | Audit whether a feature or product is ready to survive real users at scale — error handling, observability, deployment safety, resilience, and operational readiness. |
| [handoff](./skills/engineering/handoff/SKILL.md) | Turn conversation context into a spec and independently-grabbable vertical-slice issues for humans or agents. |
| [vibe-code-antipatterns](./skills/engineering/vibe-code-antipatterns/SKILL.md) | Audit a codebase for structural problems common in AI-generated code — god components, missing error handling, copy-paste duplication, inconsistent patterns, and happy-path-only logic. |

## Structure

```
skills/
├── .claude-plugin/
│   └── plugin.json
├── CLAUDE.md
├── README.md
└── skills/
    ├── product/
    │   ├── improve-product-feel/
    │   │   ├── SKILL.md
    │   │   ├── LANGUAGE.md
    │   │   ├── INTERACTION-DESIGN.md
    │   │   ├── CONTEXT-FORMAT.md
    │   │   └── ADR-FORMAT.md
    │   ├── audit-empty-states/
    │   │   ├── SKILL.md
    │   │   ├── TAXONOMY.md
    │   │   ├── SCORING-RUBRIC.md
    │   │   └── COPY-PATTERNS.md
    │   ├── error-copy-review/
    │   │   ├── SKILL.md
    │   │   ├── CLASSIFICATION.md
    │   │   ├── SCORING-RUBRIC.md
    │   │   ├── COPY-PATTERNS.md
    │   │   └── VALIDATION-MESSAGES.md
    │   ├── onboarding-friction/
    │   │   ├── SKILL.md
    │   │   ├── ASSUMPTION-TYPES.md
    │   │   ├── FRICTION-MAP-FORMAT.md
    │   │   └── REMEDIES.md
    │   ├── ai-design-polish/
    │   │   ├── SKILL.md
    │   │   ├── REFERENCE-PATTERNS.md
    │   │   ├── AUDIT-CHECKLIST.md
    │   │   ├── ELEVATION-PATTERNS.md
    │   │   └── COPY-UPGRADE.md
    │   ├── trust-building-patterns/
    │   │   ├── SKILL.md
    │   │   ├── GLOSSARY.md
    │   │   ├── TRUST-SIGNALS.md
    │   │   ├── ANTI-PATTERNS.md
    │   │   └── SCORING-RUBRIC.md
    │   └── website-teardown/
    │       ├── SKILL.md
    │       ├── DIMENSIONS.md
    │       ├── TEARDOWN-FORMAT.md
    │       ├── COMPARATIVE-TEARDOWN.md
    │       └── PATTERN-EXTRACTION.md
    ├── gtm/
        │   ├── pricing-research/
        │   │   ├── SKILL.md
        │   │   ├── GLOSSARY.md
        │   │   ├── METHODOLOGY.md
        │   │   ├── VAN-WESTENDORP.md
        │   │   ├── SEGMENTATION.md
        │   │   ├── BANDED-ANALYSIS.md
        │   │   ├── REVENUE-MIX.md
        │   │   └── REPORT-TEMPLATE.md
        │   ├── competitive-positioning/
        │   │   ├── SKILL.md
        │   │   ├── GLOSSARY.md
        │   │   ├── RESEARCH-FRAMEWORK.md
        │   │   ├── AXES.md
        │   │   └── POSITIONING-DOCUMENT.md
        │   ├── ideal-customer-profile/
        │   │   ├── SKILL.md
        │   │   ├── GLOSSARY.md
        │   │   ├── QUALIFYING-CRITERIA.md
        │   │   ├── SCORING-FRAMEWORK.md
        │   │   └── ICP-DOCUMENT.md
        │   ├── launch-brief/
        │   │   ├── SKILL.md
        │   │   ├── AUDIENCE-GRILLING.md
        │   │   ├── MESSAGE-TESTING.md
        │   │   ├── BRIEF-FORMAT.md
        │   │   └── CHANNEL-SELECTION.md
        │   └── seo-aeo/
        │       ├── SKILL.md
        │       ├── GLOSSARY.md
        │       ├── TECHNICAL-CHECKLIST.md
        │       ├── CONTENT-FRAMEWORK.md
        │       └── AEO-PATTERNS.md
        ├── security/
        │   └── security-review/
        │       ├── SKILL.md
        │       ├── GLOSSARY.md
        │       ├── VULNERABILITY-TAXONOMY.md
        │       ├── SCORING-RUBRIC.md
        │       └── REMEDIATION-PATTERNS.md
        └── engineering/
            ├── production-ready/
            │   ├── SKILL.md
            │   ├── GLOSSARY.md
            │   ├── READINESS-CHECKLIST.md
            │   ├── SCORING-RUBRIC.md
            │   └── REMEDIATION-PATTERNS.md
            ├── handoff/
            │   ├── SKILL.md
            │   ├── GLOSSARY.md
            │   ├── SPEC-FORMAT.md
            │   ├── SLICING.md
            │   └── ISSUE-FORMAT.md
            └── vibe-code-antipatterns/
                ├── SKILL.md
                ├── SMELL-TAXONOMY.md
                ├── SCORING-RUBRIC.md
                ├── REFACTORING-PLAYBOOK.md
                └── DETECTION-PATTERNS.md
```

## Creating new skills

See `CLAUDE.md` for the format requirements. Each skill needs:

1. A directory under `skills/<bucket>/`
2. A `SKILL.md` with `name` and `description` frontmatter
3. An entry in `.claude-plugin/plugin.json`
4. A reference in this README

## License

MIT
