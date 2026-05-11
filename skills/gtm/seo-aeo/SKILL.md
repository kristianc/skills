---
name: seo-aeo
description: Audit and improve search engine optimization and answer engine optimization — traditional SEO for Google rankings plus AEO for AI-powered search (ChatGPT, Perplexity, AI Overviews). Use when the user wants to improve search rankings, make content discoverable by AI, audit technical SEO, optimize for featured snippets, or ensure their site is citable by answer engines.
---

# SEO + AEO

Audit a site for both traditional search engine optimization and answer engine optimization — the emerging discipline of making content discoverable by AI-powered search (ChatGPT, Perplexity, Google AI Overviews, Bing Copilot) in addition to traditional crawlers and ranking algorithms.

The core insight: SEO and AEO are converging but have different requirements. Traditional SEO optimizes for crawlers and ranking signals. AEO optimizes for AI systems that extract, summarize, and cite content. A site needs both. Technical SEO gets you indexed. Content SEO gets you ranked. AEO gets you cited.

## Glossary

Use these terms precisely. Full definitions in GLOSSARY.md.

- **SEO** — optimizing content and structure so search engines can crawl, index, and rank it.
- **AEO** — optimizing content so AI systems can extract, summarize, and cite it with attribution.
- **E-E-A-T** — Experience, Expertise, Authoritativeness, Trustworthiness. Google's quality framework.
- **Citability** — the quality that makes a paragraph useful when an AI quotes it as an answer.
- **Structured data** — machine-readable metadata (schema.org) embedded in pages that search engines and AI systems parse directly.
- **Search intent** — what the user actually wants when they type a query (informational, transactional, navigational, commercial investigation).

## Process

### 1. Scope

Establish what is being audited: the full site, specific landing pages, a blog or docs section, or a single page. Ask the user if not obvious.

Identify the stack so technical recommendations are actionable — a Next.js site gets different advice than a WordPress site or a static HTML page. Determine what the site is trying to rank for: product keywords, informational queries, branded terms, or a specific content vertical.

Define the audience: who searches for what this site offers, and do they search on Google, ask AI, or both?

### 2. Technical audit

Walk the technical SEO fundamentals against TECHNICAL-CHECKLIST.md. For each category — indexability, meta tags, heading structure, structured data, performance, mobile, internal linking — check every applicable item against the actual code.

Use the Agent tool with `subagent_type=Explore` to read the codebase. Search for meta tags in layout files, heading patterns in page templates, structured data in JSON-LD blocks, sitemap configuration, robots.txt, image optimization settings, and link patterns.

Don't stop at the first finding per category. A site can be missing structured data AND have duplicate title tags AND lack canonical URLs on paginated content.

### 3. Content audit

Assess the content structure against CONTENT-FRAMEWORK.md. Evaluate:

- Whether content matches the search intent for target queries
- How E-E-A-T signals are embedded (or absent) in the content and site structure
- Whether the content structure supports AI extraction — clear topic sentences, self-contained paragraphs, structured lists and tables
- Whether the heading hierarchy serves as a navigable table of contents for both humans and AI
- Whether freshness signals are present and accurate

### 4. AEO audit

Evaluate the site's readiness for AI-powered search against AEO-PATTERNS.md. Check:

- Citability — can AI quote paragraphs as useful, attributed answers?
- Entity clarity — does AI understand what this site/brand IS?
- FAQ and How-to content — structured for AI extraction with proper schema?
- Comparison content — structured with honest, parseable data?
- Freshness signals — dates, update indicators, changelog references?

### 5. Present findings

Organize findings by impact — what will move the needle most for rankings and AI visibility. For each finding:

- **What's missing** — the specific gap, with reference to the checklist item
- **Why it matters** — the SEO impact, AEO impact, or both
- **What to implement** — the concrete change, with code patterns where applicable

Group into: technical fixes (usually quick), content structure improvements (medium effort), and AEO enhancements (often requires new content or markup). Ask the user which to address.

### 6. Implement

Make the code changes. Common implementations: adding meta tags to layout templates, restructuring headings, adding JSON-LD structured data blocks, creating or updating sitemaps, adding schema markup for FAQ/HowTo content, restructuring content for citability.

After implementing, verify:

- Structured data validates (Google Rich Results Test patterns)
- Meta tags are unique per page, not duplicated from a template
- Heading hierarchy is logical (no skipped levels, single H1)
- Schema markup matches the actual page content (not aspirational)

## Rules

- Every finding must include a concrete fix. An SEO audit without implementation guidance is a worry list, not an audit.
- Distinguish between fixes that affect SEO, AEO, or both. Label each finding accordingly. Many fixes serve both — structured data helps Google rich results AND AI extraction.
- Don't recommend keyword stuffing, hidden text, or manipulative link schemes. These are spam techniques that get sites penalized. Every recommendation should improve the experience for both humans and machines.
- Be specific to the stack. "Add structured data" is not actionable. "Add a JSON-LD FAQPage block in the layout component at `app/layout.tsx`" is.
- Acknowledge what's already solid. A site with strong technical SEO that needs AEO work is in a different position than a site missing the basics.
- Prioritize impact. A missing meta description on the homepage matters more than a missing meta description on a 404 page. Focus energy where traffic lives.
