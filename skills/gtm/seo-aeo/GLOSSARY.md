# GLOSSARY.md

Key terms for search engine optimization and answer engine optimization. Each defined concretely with relevance to both traditional search and AI-powered search.

---

## SEO (Search Engine Optimization)

The practice of optimizing content, structure, and technical implementation so search engines can crawl, index, and rank a site. Encompasses technical SEO (can crawlers access and understand the site?), on-page SEO (do individual pages signal relevance for target queries?), and off-page SEO (does the broader web indicate this site is authoritative?).

**SEO relevance:** The entire discipline.
**AEO relevance:** SEO is the foundation. AI systems often use search-indexed content as their source material. A site that isn't indexed well is unlikely to be cited by AI.

---

## AEO (Answer Engine Optimization)

The practice of optimizing content so AI-powered search systems — ChatGPT, Perplexity, Google AI Overviews, Bing Copilot — can extract, summarize, and cite it. Where SEO optimizes for ranking position, AEO optimizes for being the source an AI quotes when answering a question.

**SEO relevance:** AEO-optimized content (clear structure, specific answers, good schema) also tends to rank well in traditional search.
**AEO relevance:** The entire discipline. AEO is emerging alongside the shift from "ten blue links" to AI-generated answers.

---

## SERP (Search Engine Results Page)

The page a search engine returns after a query. Modern SERPs include organic results, paid ads, featured snippets, knowledge panels, People Also Ask boxes, image packs, video carousels, and AI Overviews. The composition of the SERP for a given query dictates what kind of content has a chance of visibility.

**SEO relevance:** The target. Everything in SEO aims to appear prominently on the SERP.
**AEO relevance:** AI Overviews now occupy the top of many SERPs. Being cited in the AI Overview can matter more than ranking #1 in organic results.

---

## Featured Snippet

A block at the top of Google's results that directly answers the search query, extracted from a web page. Appears as a paragraph, list, table, or video pulled from the source page. The source gets prominent placement and a link.

**SEO relevance:** "Position zero" — appears above the first organic result. Drives significant click-through for informational queries.
**AEO relevance:** Featured snippets are the precursor to AI Overviews. Content structured to win featured snippets is often the same content AI systems extract.

---

## AI Overview

Google's AI-generated answer that appears at the top of search results for many queries. Synthesizes information from multiple sources and includes citation links. Replaces or supplements featured snippets for many queries.

**SEO relevance:** Can reduce clicks to organic results (the AI answered the question directly). Being cited in the overview partially compensates.
**AEO relevance:** The primary target for AEO on Google. Being a cited source in AI Overviews is the AEO equivalent of ranking #1.

---

## Knowledge Panel

A structured information box that appears on the right side of Google results for recognized entities (companies, people, products, places). Pulls from the Knowledge Graph, Wikipedia, and structured data on the entity's own site.

**SEO relevance:** Establishes brand presence on the SERP. Provides key information (address, hours, social profiles) directly.
**AEO relevance:** Knowledge panels reflect how well search engines understand your entity. Strong entity understanding translates to better AI citation, because AI systems build on the same entity graph.

---

## Schema.org

A collaborative vocabulary for structured data markup that search engines and AI systems use to understand page content. Implemented as JSON-LD (recommended), Microdata, or RDFa embedded in HTML. Defines types like Organization, Article, Product, FAQPage, HowTo, Review, BreadcrumbList, and hundreds more.

**SEO relevance:** Enables rich results (star ratings, FAQ dropdowns, recipe cards, event listings) in search results.
**AEO relevance:** Schema markup is machine-readable by design. AI systems parse it directly to understand what a page contains, who created it, when it was updated, and what entities it describes.

---

## Structured Data

Machine-readable information embedded in a web page that describes the page's content in a format search engines and AI systems can parse. Most commonly implemented as JSON-LD blocks using schema.org vocabulary. Distinct from the visible content — structured data is metadata about the content.

**SEO relevance:** Required for rich results. Helps search engines understand content type, authorship, dates, ratings, and relationships.
**AEO relevance:** AI systems use structured data as a reliable signal for facts, dates, authorship, and entity relationships. A page with explicit structured data is easier for AI to cite accurately.

---

## E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness)

Google's quality framework for evaluating content, expanded in 2022 to add Experience. Not a direct ranking factor — it's a set of criteria that Google's quality raters use to evaluate search results, which informs algorithm development.

- **Experience** — first-hand, practical knowledge of the topic. A product review from someone who actually used the product.
- **Expertise** — depth of knowledge in the subject area. Credentials, demonstrated skill, detailed understanding.
- **Authoritativeness** — recognition from others as a go-to source. Backlinks, citations, brand reputation.
- **Trustworthiness** — accuracy, transparency, honesty. The overarching quality that the other three support.

**SEO relevance:** Content demonstrating strong E-E-A-T signals tends to rank higher, especially for YMYL (Your Money or Your Life) topics like health, finance, and legal.
**AEO relevance:** AI systems are learning to assess source credibility. Content with clear authorship, cited sources, and demonstrated expertise is more likely to be selected as an AI citation.

---

## Search Intent

The underlying goal behind a search query. Four primary types:

- **Informational** — the user wants to learn something. "What is schema markup" or "how to add structured data."
- **Navigational** — the user wants to reach a specific site. "GitHub login" or "Stripe documentation."
- **Transactional** — the user wants to complete an action. "Buy running shoes" or "sign up for Notion."
- **Commercial investigation** — the user is researching before a purchase. "Best project management tools 2025" or "Notion vs Asana."

**SEO relevance:** Content must match the intent behind the query it targets. A product page won't rank for an informational query. A blog post won't rank for a transactional query.
**AEO relevance:** AI systems are particularly strong at answering informational and commercial investigation queries. Content optimized for these intents is most likely to be cited by AI.

---

## Canonical URL

An HTML element (`<link rel="canonical" href="...">`) that tells search engines which version of a page is the authoritative one. Used when the same content is accessible at multiple URLs (www vs. non-www, with or without trailing slashes, paginated content, filtered views).

**SEO relevance:** Prevents duplicate content dilution. Consolidates ranking signals to a single URL.
**AEO relevance:** AI systems that crawl and index content respect canonical URLs to avoid citing duplicate sources.

---

## Sitemap (XML Sitemap)

An XML file that lists all important pages on a site, helping search engines discover and crawl content efficiently. Includes metadata like last modification date, change frequency, and priority.

**SEO relevance:** Ensures search engines find all important pages, especially on large sites or sites with content not well-linked internally.
**AEO relevance:** AI systems and their crawlers use sitemaps for content discovery. A well-maintained sitemap with accurate last-modified dates helps AI systems identify fresh content.

---

## Robots.txt

A text file at the site root that tells crawlers which parts of the site they can and cannot access. Controls crawl behavior but does not control indexing (use `noindex` meta tags for that).

**SEO relevance:** Prevents crawlers from wasting budget on low-value pages (admin panels, search results pages, duplicate filtered views). Can accidentally block important content if misconfigured.
**AEO relevance:** AI crawlers (GPTBot, Anthropic's crawler, PerplexityBot) check robots.txt. Blocking these crawlers prevents your content from being included in AI training and citation. Allowing them is an active choice that enables AEO.

---

## Core Web Vitals

Google's set of metrics measuring real-world user experience. Three metrics:

- **LCP (Largest Contentful Paint)** — how quickly the main content loads. Target: under 2.5 seconds.
- **FID (First Input Delay)** / **INP (Interaction to Next Paint)** — how quickly the page responds to user interaction. Target: under 200ms (INP replaced FID in 2024).
- **CLS (Cumulative Layout Shift)** — how much the page layout shifts during loading. Target: under 0.1.

**SEO relevance:** A confirmed Google ranking signal. Poor Core Web Vitals can prevent ranking in the top positions.
**AEO relevance:** Indirect. Fast, stable pages are more likely to be fully crawled and indexed, which makes them available for AI extraction.

---

## Meta Tags

HTML elements in the `<head>` that provide metadata about the page. The most SEO-relevant:

- `<title>` — the page title displayed in search results and browser tabs. 50-60 characters.
- `<meta name="description">` — the page summary displayed in search results. 150-160 characters.
- `<meta name="robots">` — crawl and index directives (index/noindex, follow/nofollow).
- `<meta property="og:...">` — Open Graph tags for social sharing previews.
- `<meta name="twitter:...">` — Twitter Card tags for Twitter/X sharing previews.

**SEO relevance:** Title tags are a direct ranking signal. Meta descriptions influence click-through rate (an indirect signal). Robot directives control indexation.
**AEO relevance:** AI systems read meta descriptions for page summaries. Well-written descriptions that accurately summarize the page help AI understand and cite content correctly.

---

## Heading Hierarchy

The H1-H6 heading structure of a page. Communicates the document's outline to both humans and machines.

- **H1** — the page's main topic. One per page.
- **H2** — major sections within the page.
- **H3-H6** — subsections within sections. Each level nests under the one above.

**SEO relevance:** Search engines use heading hierarchy to understand page structure and topic coverage. Headings containing target keywords signal relevance.
**AEO relevance:** AI systems navigate heading hierarchy like a table of contents. Clear, descriptive headings that phrase topics as questions help AI find and extract the right section for a given query.

---

## Internal Linking

Links between pages on the same site. Distributes ranking authority, helps crawlers discover content, and guides users to related information.

**SEO relevance:** Strong internal linking ensures important pages receive ranking authority from other pages on the site. Helps search engines understand site structure and page relationships.
**AEO relevance:** Internal links help AI systems understand topical relationships. A page linked from multiple relevant pages on the same site is more likely to be understood as authoritative on that topic.

---

## Topical Authority

The degree to which a site is recognized as a comprehensive, credible source on a specific topic. Built by publishing depth of content across a topic cluster — not just one page on "project management" but pages on subtopics, comparisons, tutorials, and guides.

**SEO relevance:** Sites with topical authority rank more easily for new content within that topic. Google increasingly rewards depth over breadth.
**AEO relevance:** AI systems select sources they assess as authoritative on a topic. A site with comprehensive topical coverage is more likely to be cited than a site with a single article.

---

## Citability

The quality that makes a piece of content useful when an AI system quotes it as an answer. A citable paragraph is self-contained, answers a specific question completely, includes specific data or claims, and can be attributed to a named source.

**SEO relevance:** Citable content tends to win featured snippets, which are the traditional SEO equivalent of being cited.
**AEO relevance:** The core AEO quality. AI systems prefer to cite paragraphs they can quote directly as useful answers. Content that requires context from surrounding paragraphs to make sense is less citable.

---

## Entity

A distinct, well-defined thing — a person, company, product, place, or concept — that search engines and AI systems can identify and track across the web. Google's Knowledge Graph contains billions of entities and their relationships.

**SEO relevance:** Sites associated with recognized entities get knowledge panels, richer SERP features, and stronger brand signals.
**AEO relevance:** AI systems understand the world in terms of entities. If AI recognizes your brand as an entity with clear attributes (what it does, who's behind it, what authority it has), it can cite your content with proper attribution.

---

## Knowledge Graph

Google's database of entities and their relationships — people, companies, products, places, concepts, and how they connect. Powers knowledge panels, AI Overviews, and entity understanding in search.

**SEO relevance:** Being in the Knowledge Graph means Google understands your entity. This enables knowledge panels and better understanding of your content's context.
**AEO relevance:** AI systems (including Google's) use entity graphs to understand source credibility and context. A brand that exists as a clear entity in the knowledge graph is more citable.

---

## Crawl Budget

The number of pages a search engine will crawl on your site within a given time period. Determined by your site's size, authority, server speed, and how efficiently the site is structured.

**SEO relevance:** For large sites (10,000+ pages), crawl budget determines how quickly new or updated content gets indexed. Wasting budget on low-value pages (duplicate content, parameter-heavy URLs) means important pages get crawled less often.
**AEO relevance:** AI crawlers also have crawl budgets. A well-structured site with clean URLs and a maintained sitemap ensures AI crawlers focus on the content that matters.

---

## Indexability

Whether a page can be found, crawled, and added to a search engine's index. A page that isn't indexed doesn't exist in search results — regardless of how good its content is. Affected by robots.txt, noindex directives, canonical tags, crawl errors, and internal linking.

**SEO relevance:** The prerequisite to ranking. If a page isn't indexed, it can't rank.
**AEO relevance:** The prerequisite to being cited. If a page isn't in the indices that AI systems draw from, it won't be cited.
