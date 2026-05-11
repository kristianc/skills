# AEO-PATTERNS.md

Answer engine-specific patterns that go beyond traditional SEO. This is the reference for the AEO audit step (Step 4). Each pattern: what it is, why AI systems use it, how to implement it, how to verify it's working, and common mistakes.

---

## 1. Citability

Writing content so AI systems can quote it with attribution. The core AEO quality.

### What citability means

A piece of content is citable when an AI system can extract a paragraph, quote it as an answer to a user's question, and the paragraph makes sense on its own — no surrounding context needed, no ambiguity about what it's saying or who said it.

**The citability test:** Take any paragraph from your content. Imagine an AI shows it to a user who asked a question. Does the paragraph:

1. Answer a specific question completely?
2. Make sense without the paragraphs before and after it?
3. Include specific information (not vague generalizations)?
4. Have a clear source the AI can attribute? (author, organization, publication)

If yes to all four, the paragraph is citable.

### Why AI systems prefer citable content

AI systems generating answers need to select source material that:

- Answers the query directly (saves the AI from having to synthesize across multiple paragraphs)
- Can be quoted with attribution (the AI can say "according to [source]")
- Contains specific claims (vague content doesn't help the user)
- Is self-contained (the AI can't reproduce the surrounding context)

Content that forces AI to paraphrase heavily, synthesize across many paragraphs, or strip out context-dependent references is less likely to be selected as a citation.

### How to implement citable content

**Self-contained paragraphs:**
Each paragraph should answer one question or make one point completely. Avoid starting paragraphs with "However," "Additionally," or "As mentioned above" — these signal dependence on context.

**Named sources and authorship:**
Every page should have a visible author or organization attribution. AI systems cite "according to [Name] at [Organization]" — if neither is present, the content is harder to attribute.

Implementation:
```html
<article>
  <header>
    <h1>How to Add Structured Data to Your Site</h1>
    <p class="author">By <a href="/team/jane-smith">Jane Smith</a>, Senior Engineer at Acme Corp</p>
    <time datetime="2025-03-15">March 15, 2025</time>
  </header>
  <!-- content -->
</article>
```

Corresponding Article schema with author details (see TECHNICAL-CHECKLIST.md).

**Specific data points and claims:**
Replace vague statements with specific ones. Instead of "many websites use structured data," write "over 35 million websites use schema.org markup (source: Web Almanac 2024)." Specific claims with sources are more citable than general statements.

**Short, direct answers near the question:**
When content addresses a specific question (especially in FAQ or how-to format), put the direct answer immediately after the question — in the first sentence of the response, not buried in the third paragraph.

### Common mistakes

- **Writing for SEO length, not AEO clarity.** A 3,000-word post with the answer buried in paragraph 12 is worse for AEO than a 500-word post with the answer in the first paragraph.
- **Over-reliance on context.** "As we discussed in the section above..." is invisible to AI extracting a single section.
- **Anonymous content.** Content with no author attribution is less citable. AI systems prefer content they can attribute.
- **Hedge-heavy writing.** "It could potentially be argued that in some cases..." is uncitable. State your position clearly.

---

## 2. Entity Establishment

Making it clear to AI systems what your site, brand, and authors ARE — so AI can cite you accurately and with confidence.

### What entity establishment means

AI systems understand the world as a graph of entities (people, companies, products, concepts) and their relationships. For AI to cite your content, it needs to understand:

- What your organization is (company, publication, nonprofit, etc.)
- What it does (the domain of expertise)
- Who is behind it (founders, authors, team)
- What authority they have (credentials, experience, recognition)

This is the foundation that makes all other AEO patterns effective. Without entity clarity, AI may cite your content but attribute it poorly, or not cite it at all because the source is unclear.

### Why AI systems need entity clarity

When an AI generates an answer citing multiple sources, it evaluates each source's credibility. A source with a clear entity identity ("Acme Corp, a developer tools company founded in 2019 by Jane Smith, former Google engineer") is more trustworthy than an anonymous blog with no about page.

AI systems build entity understanding from:
- Schema.org Organization markup on the site
- About pages describing the organization and team
- Wikipedia and Wikidata entries for the brand
- Consistent mentions across the web (name, description, attributes)
- Author pages with credentials and published work

### How to implement entity establishment

**Organization schema (required):**

On the homepage, include comprehensive Organization schema:

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Acme Corp",
  "url": "https://acme.com",
  "logo": "https://acme.com/logo.png",
  "description": "Developer tools for infrastructure automation.",
  "foundingDate": "2019",
  "founder": {
    "@type": "Person",
    "name": "Jane Smith",
    "jobTitle": "CEO",
    "url": "https://acme.com/team/jane"
  },
  "sameAs": [
    "https://twitter.com/acmecorp",
    "https://linkedin.com/company/acmecorp",
    "https://github.com/acmecorp",
    "https://en.wikipedia.org/wiki/Acme_Corp"
  ]
}
```

**About page (required):**

A detailed about page that explains what the organization does, who's behind it, and what qualifies them. This is one of the first pages AI systems check when evaluating source credibility.

What to include: company description, founding story (briefly), team members with credentials, domain of expertise, notable achievements or customers, contact information.

**Author pages (recommended):**

For sites that publish content, each author should have a page with:
- Full name
- Photo
- Bio with relevant credentials and experience
- Links to published work (on-site and elsewhere)
- Social profiles
- Person schema markup

```json
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Jane Smith",
  "jobTitle": "Senior Engineer",
  "worksFor": {
    "@type": "Organization",
    "name": "Acme Corp"
  },
  "url": "https://acme.com/team/jane",
  "sameAs": [
    "https://twitter.com/janesmith",
    "https://linkedin.com/in/janesmith",
    "https://github.com/janesmith"
  ]
}
```

**Consistent NAP (Name, Address, Phone):**

Use the exact same business name, address, and phone number across your site, Google Business Profile, social profiles, and directory listings. Inconsistencies make it harder for AI to connect these mentions to a single entity.

**Wikipedia and Wikidata presence:**

For established brands, a Wikipedia article and Wikidata entry significantly boost entity recognition. These are the primary sources for Google's Knowledge Graph. Meeting Wikipedia's notability guidelines requires third-party coverage — press articles, industry recognition, notable customers.

### How to verify entity establishment

- Search your brand name on Google. Does a knowledge panel appear?
- Ask ChatGPT or Perplexity "What is [your brand]?" Does it return accurate information?
- Check Google's Knowledge Graph Search API for your entity
- Verify Organization schema with Google's Rich Results Test

### Common mistakes

- **No about page.** The most basic entity establishment signal, and it's missing on a surprising number of sites.
- **Anonymous authorship.** Content published under "Admin" or "Team" instead of a named author.
- **Inconsistent brand name.** "Acme Corp" on the site, "ACME" on Twitter, "Acme Corporation" on LinkedIn. Pick one canonical name.
- **Missing social profile links.** AI systems use sameAs links to connect your site to your presence on other platforms.

---

## 3. FAQ Optimization

Writing FAQs that AI systems extract as direct answers.

### What makes an FAQ extractable

AI systems look for clear question-and-answer pairs where the answer directly and completely addresses the question. The combination of FAQPage schema markup and well-structured visible content makes FAQ sections one of the highest-value AEO patterns.

### How FAQ queries differ for AI

People phrase questions differently when asking AI than when typing into Google:

- **Google:** "structured data SEO"
- **AI:** "What is structured data and why does it matter for SEO?"

AI queries are conversational, specific, and phrased as complete questions. FAQ questions should match this conversational phrasing.

### How to implement FAQ optimization

**Question phrasing:**
Use the actual questions your audience asks. Pull from:
- Google's "People Also Ask" for your target keywords
- Customer support tickets (the literal questions people email)
- Forum discussions (Reddit, Stack Overflow, community forums)
- AI chat logs if available

Write questions in natural language, not keyword-stuffed:

**Good:** "How long does it take to set up structured data?"
**Bad:** "Structured data setup time implementation duration"

**Answer structure:**
Start with the direct answer in the first sentence. Follow with supporting detail. End with a specific example or next step.

**Good:**
> Q: How long does it take to set up structured data?
>
> A: Adding basic structured data (Organization and Article schema) to a typical site takes 2-4 hours. This includes identifying the right schema types for your content, writing the JSON-LD blocks, adding them to your templates, and validating with Google's Rich Results Test. For a CMS like WordPress, plugins can reduce this to under an hour.

**Bad:**
> Q: How long does it take to set up structured data?
>
> A: It depends on many factors. First, you need to understand what structured data is. Structured data is a standardized format for providing information about a page... [200 words before answering the question]

**FAQPage schema markup:**

Add JSON-LD markup that matches the visible FAQ content exactly:

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "How long does it take to set up structured data?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Adding basic structured data (Organization and Article schema) to a typical site takes 2-4 hours."
      }
    }
  ]
}
```

### SEO FAQ vs. AEO FAQ

**SEO FAQ** targets featured snippets in Google. Optimized for keyword matching and concise answers that fit in a snippet box. Answer length: 40-60 words.

**AEO FAQ** targets AI extraction. Optimized for completeness and citability. Answer length: 50-100 words — complete enough to be a standalone answer, short enough to be quoted.

The overlap is significant. A well-written FAQ answer of 50-70 words serves both purposes. Start with the direct answer (for featured snippets), follow with supporting detail (for AI citation).

### Common mistakes

- **Questions no one asks.** Internal jargon or questions that serve the company, not the user. "Why is our product the best?" is not a real question.
- **Answers that don't answer.** Responses that dodge the question and redirect to "contact us" or "it depends."
- **Schema that doesn't match visible content.** The FAQ in the JSON-LD must match what's on the page. Mismatches are a policy violation.
- **Too many FAQs on one page.** Google recommends no more than 10-15 FAQ items per page. More than that and the page looks spammy.

---

## 4. How-To Optimization

Step-by-step content that AI systems extract for instructional queries.

### What makes a how-to extractable

AI systems love step-by-step content because it maps directly to "how do I..." queries. Numbered steps with clear outcomes, concrete actions, and optional tool/material lists are highly extractable.

### How to implement how-to optimization

**Numbered steps with clear outcomes:**
Each step should be a discrete action with a verifiable outcome. The reader should know when they've completed the step.

**Good:**
> Step 1: Add the JSON-LD script tag to your page's `<head>` section. After adding it, view the page source to confirm the script tag is present.

**Bad:**
> Step 1: Think about what structured data you need. Consider the various options and their implications for your site's SEO strategy.

**Tool and material lists:**
When a how-to requires specific tools, list them upfront:

```json
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "How to Add FAQ Schema to Your Website",
  "totalTime": "PT30M",
  "tool": [
    { "@type": "HowToTool", "name": "Text editor or IDE" },
    { "@type": "HowToTool", "name": "Google Rich Results Test" }
  ],
  "step": [
    {
      "@type": "HowToStep",
      "name": "Identify your FAQ content",
      "text": "Find the questions and answers on your page that you want to mark up with FAQPage schema."
    },
    {
      "@type": "HowToStep",
      "name": "Write the JSON-LD block",
      "text": "Create a JSON-LD script with @type FAQPage containing your question-answer pairs."
    }
  ]
}
```

**Time estimates:**
Include `totalTime` in the HowTo schema and mention the time estimate in the visible content. "This takes about 30 minutes" is useful information for both humans and AI.

### Common mistakes

- **Steps that aren't discrete actions.** "Understand the fundamentals" is not a step. "Run `npm install schema-dts`" is a step.
- **Missing prerequisites.** What does the reader need to know or have before starting? List it.
- **No verification.** Each step should indicate how the reader knows they did it correctly.
- **Schema without visible steps.** HowTo schema must match visible content on the page.

---

## 5. Comparison and "Best Of" Content

How AI systems pull from comparison content for commercial investigation queries.

### Why comparison content is high-value for AEO

"Best X for Y" and "X vs Y" are among the most common AI queries. Users ask AI for recommendations because they want a synthesized answer, not 10 blue links. If your content is the source AI cites for these comparisons, you capture significant mindshare.

### How to structure comparison content

**Structured tables with clear criteria:**

Tables are the most extractable format for comparison content. Each column should be a clear evaluation criterion.

| Tool | Price | Free tier | API | Best for |
|------|-------|-----------|-----|----------|
| Tool A | $29/mo | Yes, 1000 requests | REST | Small teams |
| Tool B | $49/mo | No | GraphQL | Enterprises |

Ensure:
- Column headers clearly describe what's being compared
- Data is consistent across rows (don't mix "Yes/No" with "Available")
- Include a "Best for" or "Verdict" column that gives specific recommendations

**Honest pros and cons:**

AI systems are getting better at detecting promotional bias. Content that honestly assesses weaknesses alongside strengths is more citable than pure promotion.

**Good:** "Tool A's free tier is generous, but the API rate limits can be restrictive for production use."
**Bad:** "Tool A is the best option for everyone with absolutely no downsides."

One-sided content gets cited less because AI systems prefer balanced sources. If you're comparing your own product to competitors, acknowledge where competitors are stronger — this paradoxically makes you more citable because the AI trusts your assessment.

**Evaluation criteria that match how people decide:**

Don't compare on 47 criteria. Compare on the 5-7 criteria that actually drive the buying decision for your audience: price, ease of use, specific features, support quality, integration ecosystem.

### Common mistakes

- **Comparing only on criteria where you win.** AI (and readers) notice this. Include criteria where competitors are strong.
- **Outdated comparisons.** Comparison content with last year's pricing or deprecated features loses credibility. Include a "last updated" date and refresh regularly.
- **No clear recommendation.** "It depends" is not a helpful conclusion. Provide specific recommendations for specific use cases.
- **Unstructured comparisons.** Prose-only comparisons without tables are harder for AI to extract and harder for humans to scan.

---

## 6. Content Freshness for AI

How AI systems weight recency and how to signal it.

### How AI weights recency

AI systems apply recency weighting based on the query type:

- **Time-sensitive queries** ("best tools 2025," "current pricing for X"): Heavy recency weighting. Undated or old content is deprioritized.
- **Evergreen queries** ("what is SQL injection," "how does DNS work"): Moderate recency weighting. Recent content preferred but old authoritative content still cited.
- **Historical queries** ("who invented the telephone"): No recency weighting. The answer doesn't change.

For most commercial and technical content, recency matters. A dated, recently-updated article outperforms an undated article, even if the undated one is objectively better.

### How to signal freshness

**Visible dates on every content page:**
- Publish date in a visible, standardized format
- "Last updated" date when content has been revised
- Both dates should be in the HTML (not just rendered by JavaScript, which some crawlers miss)

**Date schema in structured data:**

```json
{
  "datePublished": "2025-01-15",
  "dateModified": "2025-04-20"
}
```

Use ISO 8601 format. Ensure `dateModified` is only updated when meaningful changes are made — updating it without changing content is detectable and penalized.

**Changelog references:**
For content that's been substantially updated, note what changed:

> *Last updated April 20, 2025: Updated pricing comparison to reflect Tool B's new enterprise tier. Added section on AI Overview optimization.*

This serves both readers (who know what's new) and AI systems (which can assess the scope of the update).

**"As of" qualifiers on time-sensitive claims:**
When stating facts that may change, qualify them: "As of May 2025, Tool A costs $29/month." This helps AI systems assess whether the information is current and signals that you're aware of the temporal nature of the claim.

### Common mistakes

- **No dates at all.** Undated content is the single biggest AEO mistake for commercial and technical content. AI systems can't assess freshness and default to assuming staleness.
- **Fake freshness.** Changing `dateModified` without changing content. Search engines and AI systems can detect this by comparing cached versions.
- **Year in the title without updates.** "Best Tools 2025" published in 2024 and never updated. The year in the title sets an expectation of recency that the content doesn't deliver.
- **Ignoring content decay.** Content published and never revisited. Statistics go stale, links break, recommendations become outdated. A content refresh schedule is essential.
