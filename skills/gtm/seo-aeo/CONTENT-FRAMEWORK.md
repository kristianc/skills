# CONTENT-FRAMEWORK.md

How to structure content for both search engines and answer engines. This is the reference for the content audit step (Step 3). Covers search intent alignment, E-E-A-T signals, content structure for AI extraction, heading strategy, and freshness signals.

---

## 1. Search Intent Alignment

Content that doesn't match intent won't rank — regardless of how good the technical SEO is.

### Understanding the four intent types

Every search query has an underlying intent. The content format must match the intent, or the page won't rank no matter how optimized it is.

**Informational — "I want to learn"**
Queries like "what is structured data," "how to add schema markup," "SEO vs AEO." The user wants to understand something. The right format: explanatory content, guides, tutorials, definitions, comparisons.

What to check: Does the content thoroughly answer the implied question? Does it go beyond the basic answer to address follow-up questions? Is it structured so someone can find the specific answer they need (via headings, table of contents)?

**Navigational — "I want to go there"**
Queries like "Stripe docs," "GitHub login," "Notion pricing." The user wants a specific page on a specific site. The right format: be the page they're looking for, with a clear title and description.

What to check: Does the site's own branded content rank for navigational queries about it? Are important pages titled clearly enough to be found?

**Transactional — "I want to do something"**
Queries like "buy running shoes," "sign up for Notion," "download VS Code." The user wants to complete an action. The right format: product pages, sign-up flows, download pages with clear CTAs.

What to check: Is the path from search to action short and clear? Does the landing page match the transactional intent (not an informational blog post)?

**Commercial investigation — "I'm researching before deciding"**
Queries like "best project management tools 2025," "Notion vs Asana," "is Stripe worth it." The user is comparing options before a purchase. The right format: comparison content, reviews, "best of" lists, feature breakdowns.

What to check: Does the content compare honestly? Are there structured comparison tables? Is the content recent (dated within the last year)?

### Diagnosing intent mismatches

A common failure mode: creating blog posts for transactional queries ("Buy our product — here's a 2,000-word article about why") or product pages for informational queries (someone searches "what is X" and lands on a pricing page).

How to check: For each target keyword, look at what currently ranks on the first page of Google. If the top results are all blog posts, a product page won't rank. If the top results are all product pages, a blog post won't rank. Match the format to what's already succeeding.

---

## 2. E-E-A-T Signals

Google's quality framework. Not a direct ranking factor — it's what quality raters evaluate, which informs algorithm development. For AEO, E-E-A-T signals help AI systems assess source credibility.

### Experience — first-hand knowledge

**What it looks like in content:**
- Personal accounts of using a product, building a feature, or solving a problem
- "We built this" or "we tested this" language (not just "it is said that")
- Screenshots, photos, or data from actual usage
- Specific details that could only come from experience (error messages encountered, workarounds discovered, actual performance numbers)

**What to check:** Does the content read like someone who did the thing, or someone who read about the thing? First-hand experience produces specific details. Second-hand content produces generalities.

**How to embed it:** Add author bios with relevant experience. Include original data, screenshots, or case details. Reference specific experiences ("when we migrated from X to Y, we discovered...").

**SEO impact:** High for review content, tutorials, and YMYL topics.
**AEO impact:** AI systems increasingly weight first-hand accounts over aggregated summaries.

### Expertise — depth of knowledge

**What it looks like in content:**
- Detailed technical explanations with correct terminology
- Nuanced positions (not just "X is good" but "X is good for Y because Z, but not for W because...")
- Content that addresses edge cases and exceptions, not just the happy path
- References to established frameworks, research, or standards

**What to check:** Does the content demonstrate understanding beyond the surface level? Would an expert in the field find inaccuracies or oversimplifications? Does it cite sources when making claims?

**How to embed it:** Ensure content is written or reviewed by someone with subject matter expertise. Add depth — cover edge cases, exceptions, and nuances. Cite sources for claims. Use correct terminology.

**SEO impact:** High for technical content, health, finance, and legal topics.
**AEO impact:** AI systems prefer to cite content that demonstrates expertise — specific, accurate, and nuanced.

### Authoritativeness — recognition from others

**What it looks like on a site:**
- Backlinks from other authoritative sites in the same domain
- Mentions and citations from recognized publications
- Awards, certifications, or industry recognition
- A body of published work on the topic (topical authority)
- Brand recognition — people search for the brand by name

**What to check:** Does the site have depth of content across the topic (topical authority)? Are other sites linking to this content? Is the brand recognized in its space?

**How to build it:** Publish comprehensive content across a topic cluster. Earn links through original research, tools, or insights. Participate in the industry (speaking, publishing, contributing to standards).

**SEO impact:** High. Authoritative sites rank more easily for new content in their domain.
**AEO impact:** AI systems select authoritative sources for citation. A site known as a go-to resource for a topic is cited more often.

### Trustworthiness — accuracy and transparency

**What it looks like on a site:**
- Accurate, up-to-date information (no outdated claims)
- Clear authorship (not anonymous)
- Editorial standards (corrections policy, fact-checking)
- Transparent business information (who runs this, where are they, how to contact)
- HTTPS, privacy policy, clear data handling

**What to check:** Is information accurate and current? Are claims backed by sources? Is the author identified? Is the business behind the site transparent? Are there any deceptive practices (misleading headlines, hidden costs, fake reviews)?

**How to embed it:** Add author bylines and bios. Cite sources for claims. Include a corrections policy. Keep content updated. Be transparent about conflicts of interest.

**SEO impact:** The overarching E-E-A-T quality — Google's framework lists Trustworthiness as the most important.
**AEO impact:** AI systems are learning to assess trustworthiness. Transparent, accurate, well-attributed content is safer for AI to cite.

---

## 3. Content Structure for AI Extraction

How to write content that AI systems can extract as useful answers.

### The citable paragraph

The atomic unit of AEO-optimized content. A citable paragraph:

1. **Starts with a topic sentence** that states the main point
2. **Follows with supporting detail** — explanation, evidence, or context
3. **Ends with a specific example or data point** that makes the answer concrete

**Optimal length:** 2-4 sentences. Long enough to be a complete answer, short enough to be quoted. If an AI cited this paragraph as the answer to a question, would a reader find it useful and complete?

**Good:**
> Schema.org is a collaborative vocabulary for structured data markup that search engines use to understand page content. Implemented as JSON-LD embedded in HTML, it defines types like Article, Product, and FAQPage that enable rich search results. Over 35 million websites use schema.org markup, making it the de facto standard for structured data on the web.

This paragraph answers "what is schema.org" completely. An AI can quote it as-is.

**Bad:**
> As we discussed earlier, there are many ways to add structured data to your site. Schema.org is one of them. It's quite useful for SEO. Let's look at how it works in the next section.

This paragraph references context ("as we discussed earlier"), makes vague claims ("quite useful"), and defers content ("next section"). An AI can't quote it as a standalone answer.

### Lists and tables as structured facts

Lists and tables encode information in a format both humans and machines can parse. AI systems frequently extract list items and table rows as structured answers.

**When to use lists:**
- Steps in a process (numbered)
- Features, benefits, or requirements (bulleted)
- Options or alternatives (bulleted with brief descriptions)

**When to use tables:**
- Comparisons (products, tools, approaches)
- Specifications (feature matrices, pricing tiers)
- Reference data (parameters, settings, options)

**Good table structure:**
- Clear column headers that describe what's compared
- Consistent data in cells (not mixing numbers and text arbitrarily)
- Enough rows to be useful but not so many that the comparison is meaningless

AI systems extract tables as structured data. A well-formatted comparison table is one of the most extractable content types for "best X" or "X vs Y" queries.

### Definition-style content

For concepts and terms, write definitions that follow a consistent pattern:

**Pattern:** "[Term] is [brief definition]. [What it does/why it matters]. [How it relates to the broader context]."

This pattern makes definitions extractable by AI and wins featured snippets in traditional search. It's especially important for industry terminology, product categories, and technical concepts.

### Self-contained sections

Each major section (H2 level) should be understandable on its own, without requiring the reader to have read previous sections. This doesn't mean eliminating narrative flow — it means ensuring each section can serve as a standalone answer if AI extracts it.

How to check: Read each H2 section in isolation. Does it make sense? Or does it depend on context established in a previous section?

---

## 4. Heading Strategy

Headings serve three audiences: human readers scanning the page, search engines determining relevance, and AI systems navigating to the right section for a specific query.

### Headings as questions

For informational content, phrase H2 and H3 headings as the questions your audience asks. This matches how people search (especially in AI chat interfaces where queries are conversational).

**Good:**
- H2: "What is answer engine optimization?"
- H2: "How does AEO differ from traditional SEO?"
- H2: "How do I add structured data to my site?"

**Acceptable:**
- H2: "Answer Engine Optimization Explained"
- H2: "AEO vs Traditional SEO"
- H2: "Adding Structured Data"

**Poor:**
- H2: "Overview"
- H2: "Key Differences"
- H2: "Implementation"

The "poor" examples are headings that could appear on any page about any topic. They don't contain the keywords a searcher would use, and they don't help AI match the section to a query.

### The H1 -> H2 -> H3 cascade as a navigable outline

The heading hierarchy should read as a table of contents that communicates the page's complete structure:

```
H1: How to Optimize Your Site for AI-Powered Search
  H2: What is answer engine optimization?
    H3: How AEO differs from traditional SEO
    H3: Why AEO matters in 2025
  H2: How do you make content citable by AI?
    H3: Writing citable paragraphs
    H3: Using structured data for AI extraction
    H3: FAQ and how-to optimization
  H2: How do you measure AEO success?
    H3: Tracking AI citations
    H3: Monitoring featured snippet performance
```

This outline tells both humans and AI exactly what the page covers and where to find each topic.

### Front-loading headings with keywords

Put the most important keyword or concept at the beginning of the heading, not the end. People (and machines) scan from left to right.

**Good:** "Structured data: how to add schema.org markup"
**Acceptable:** "How to add schema.org structured data to your site"
**Poor:** "A guide to the latest approaches for implementing structured data"

---

## 5. Freshness Signals

AI systems and search engines weight content recency. Fresh content with visible dates outperforms undated content for time-sensitive queries.

### Publish dates and last-updated dates

Every piece of content should show when it was published and when it was last updated. This serves both readers (who need to assess whether the information is current) and machines (which use dates to weight recency).

**How to implement:**
- Visible publish date on every content page
- Visible "last updated" date when content has been revised
- Corresponding `datePublished` and `dateModified` in Article/BlogPosting schema
- Accurate dates — don't update `dateModified` without actually changing content (search engines detect this)

### Content refresh strategy

Content that was accurate when published may become outdated. A refresh strategy keeps content current:

- **Evergreen content:** Review annually. Update statistics, check for broken links, verify that recommended practices haven't changed.
- **Time-sensitive content:** Update when the information changes. "Best tools for 2025" needs updating in 2026.
- **Technical content:** Update when the technology changes. Version-specific documentation needs updating with new releases.

When refreshing content, update both the content and the `dateModified` in the structured data. Note what was changed (an "updated" note at the top of the article or a changelog section at the bottom).

### Why AI systems weight recency

AI systems are trained to prefer recent information for queries where recency matters. A comparison post dated 2023 will lose citations to a comparison post dated 2025 for "best X in 2025" queries, even if the 2023 post is better written.

Visible freshness signals — dates on the page, "last updated" notices, changelog references — are the primary way AI systems assess whether content is current. Undated content is assumed to be potentially stale.
