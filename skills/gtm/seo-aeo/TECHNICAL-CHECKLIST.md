# TECHNICAL-CHECKLIST.md

The deep reference for the technical audit step (Step 2). Each category lists the items to check, what to look for in code, what good looks like, what bad looks like, how to fix it, and whether it affects SEO, AEO, or both. Walk every applicable category for the site being audited.

---

## 1. Indexability

Can search engines and AI crawlers find, access, and index the important pages?

### Robots.txt configured correctly

**What it means:** The `robots.txt` file at the site root tells crawlers which pages to access and which to skip. A misconfigured robots.txt can block important content or waste crawl budget on low-value pages.

**How to check in code:** Look for `robots.txt` or `public/robots.txt` in the project root. In Next.js, check for `app/robots.ts` or `app/robots.txt`. In frameworks with server config, check for robots.txt generation in build scripts.

**Good:** Important pages are accessible. Low-value pages (admin, search results, parameter-heavy URLs) are blocked. AI crawlers (GPTBot, anthropic-ai, PerplexityBot) are explicitly allowed if the user wants AEO visibility.

**Bad:** `Disallow: /` blocking everything. No robots.txt at all (not harmful but a missed opportunity). AI crawlers blocked when the user wants AI citation. Important content paths accidentally blocked.

**Fix:** Create or update robots.txt. Allow all important paths. Block admin, internal search results, and duplicate filtered views. Explicitly list AI crawler user agents with appropriate access.

**Affects:** SEO + AEO

### XML sitemap exists and is accurate

**What it means:** An XML sitemap lists all important pages with their last modification dates, helping crawlers discover content efficiently.

**How to check in code:** Look for `sitemap.xml` in the public directory, or sitemap generation in build scripts. In Next.js, check for `app/sitemap.ts`. In CMS-based sites, check for sitemap plugins. Verify: does it include all important pages? Are `<lastmod>` dates accurate (not all set to the same date)?

**Good:** Sitemap exists, includes all important pages, excludes noindex pages, has accurate `<lastmod>` dates, and is referenced in robots.txt (`Sitemap: https://example.com/sitemap.xml`).

**Bad:** No sitemap. Sitemap exists but is stale (last generated months ago with no auto-update). Sitemap includes noindex pages or returns 404 URLs. All `<lastmod>` dates are the same (search engines stop trusting the dates).

**Fix:** Generate a sitemap automatically as part of the build process. Include only indexable pages. Set `<lastmod>` to the actual last modification date of each page's content. Reference the sitemap in robots.txt.

**Affects:** SEO + AEO

### Canonical URLs set

**What it means:** Each page declares its canonical URL via `<link rel="canonical" href="...">`, telling search engines which version is authoritative when the same content is accessible at multiple URLs.

**How to check in code:** Search for `rel="canonical"` or `canonical` in layout/head components. Check that canonical URLs are absolute (not relative), consistent (www vs. non-www, trailing slashes), and point to the correct version. For paginated content, check that page 2, 3, etc. either self-canonicalize or point to a view-all page.

**Good:** Every page has a canonical URL. It matches the preferred URL format. Paginated pages self-canonicalize. Filtered/sorted views point to the unfiltered canonical.

**Bad:** No canonical tags. Canonical URLs are relative (`/about` instead of `https://example.com/about`). Canonical points to a different page (copy-paste error). Every page canonicalizes to the homepage.

**Fix:** Add canonical tags to the layout template. Use absolute URLs. For dynamic pages, generate the canonical from the current page's URL. For filtered views, canonical to the unfiltered version.

**Affects:** SEO (primary) + AEO (prevents duplicate citations)

### No accidental noindex on important pages

**What it means:** The `<meta name="robots" content="noindex">` directive or `X-Robots-Tag: noindex` HTTP header tells search engines not to index a page. Sometimes added during development and never removed, or applied too broadly via a template.

**How to check in code:** Search for `noindex` in meta tags, HTTP headers, and middleware. Check if there's a staging/production toggle — a common pattern is `noindex` in staging that accidentally deploys to production. Check the layout template — a noindex in the base layout affects every page.

**Good:** Only pages that should be excluded from search (admin, internal tools, thank-you pages, preview drafts) have noindex. All important content pages are indexable.

**Bad:** Noindex in the base layout (blocks everything). Noindex left from staging environment. Noindex on important pages (pricing, features, blog posts).

**Fix:** Remove accidental noindex directives. Add environment-based logic: noindex in staging/preview, index in production. Audit all pages with noindex to confirm they should be excluded.

**Affects:** SEO + AEO

### Hreflang for multilingual sites

**What it means:** The `hreflang` attribute tells search engines which language and regional version of a page to show to which users. Required for sites with content in multiple languages or regional variations (en-US vs. en-GB).

**How to check in code:** Search for `hreflang` in link tags or HTTP headers. For each page with translations, verify: every language version links to every other version (including itself), the hreflang values use correct language codes (ISO 639-1) and optional region codes (ISO 3166-1 Alpha-2), and there's an `x-default` fallback.

**Good:** Every translated page includes hreflang links to all versions. Codes are correct. `x-default` points to the language selector or primary language.

**Bad:** Hreflang links are incomplete (page A links to page B but B doesn't link back to A). Wrong language codes. Missing `x-default`. Hreflang on monolingual sites (unnecessary).

**Fix:** Generate hreflang tags automatically from the site's locale configuration. Ensure bidirectional linking. Add `x-default` to the primary language version.

**Affects:** SEO (primarily international SEO)

---

## 2. Meta Tags

Does each page have unique, descriptive metadata that communicates its content to search engines and social platforms?

### Unique title tags (50-60 characters)

**What it means:** The `<title>` element is the single most important on-page SEO element. It appears in search results, browser tabs, and social shares. Must be unique per page and describe the page's specific content.

**How to check in code:** Search for `<title>` in layout and page components. Check: is the title hardcoded in the layout (same for every page) or dynamic per page? Does it include the page's primary keyword? Is it within 50-60 characters? Does it follow a consistent format (e.g., "Page Topic | Brand Name")?

**Good:** Every page has a unique title that describes its specific content. Primary keyword appears near the beginning. Brand name is appended (not prepended). Length is 50-60 characters.

**Bad:** Same title on every page ("Welcome to Our Site"). Title is just the brand name. Title exceeds 60 characters (gets truncated in search results). Title is generic ("Home", "Page 1"). No title tag at all.

**Fix:** Add dynamic title generation per page. Follow the pattern: "[Primary keyword/topic] [qualifying detail] | [Brand]". For Next.js: use `metadata` export or `generateMetadata`. For other frameworks: set in the page's head section.

**Affects:** SEO (direct ranking signal) + AEO (AI reads titles to understand page topic)

### Unique meta descriptions (150-160 characters)

**What it means:** The `<meta name="description">` provides a summary displayed in search results below the title. Not a direct ranking factor, but influences click-through rate, which affects rankings indirectly.

**How to check in code:** Search for `meta` with `name="description"` in head components. Check: is it unique per page? Does it accurately summarize the page content? Is it within 150-160 characters? Does it include a call to action or value proposition?

**Good:** Every page has a unique description that summarizes its specific content. Includes the primary keyword naturally. Contains a reason to click (benefit, data point, or unique angle). 150-160 characters.

**Bad:** Same description on every page. No description (Google generates one from page content — often poorly). Description is a keyword list. Description doesn't match page content.

**Fix:** Add unique descriptions per page. Summarize what the user will find, include the primary keyword, and give a reason to click. For programmatic pages (product listings, blog posts), generate descriptions from content fields.

**Affects:** SEO (click-through rate) + AEO (AI uses descriptions as page summaries)

### Open Graph tags for social sharing

**What it means:** OG tags (`og:title`, `og:description`, `og:image`, `og:url`, `og:type`) control how the page appears when shared on social platforms, messaging apps, and AI citation previews.

**How to check in code:** Search for `property="og:` in head components. Check: are `og:title`, `og:description`, `og:image`, and `og:url` present? Is `og:image` a valid URL to an image with recommended dimensions (1200x630px)? Does it fall back to the page title and description if OG-specific values aren't set?

**Good:** All four core OG tags present on every page. OG image is a high-quality, correctly sized image (1200x630). OG title and description may differ from the HTML title/description to be optimized for social context.

**Bad:** No OG tags (social shares show whatever the platform guesses). OG image is missing or broken. OG tags are the same on every page.

**Fix:** Add OG tags to the layout template with per-page overrides. Generate or assign OG images for key pages. At minimum, inherit from the page's title and description.

**Affects:** SEO (indirect — social sharing drives traffic and links) + AEO (AI citation previews use OG data)

### Twitter Card meta

**What it means:** Twitter/X Card tags (`twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`) control how links appear on Twitter/X. Similar to OG tags but specific to that platform.

**How to check in code:** Search for `name="twitter:` in head components. At minimum, `twitter:card` should be set to `summary_large_image` for most pages.

**Good:** `twitter:card`, `twitter:title`, `twitter:description`, and `twitter:image` present. Falls back to OG tags if not set explicitly.

**Bad:** No Twitter Card tags. Many platforms fall back to OG tags, so this is lower priority than OG — but explicit Twitter Card tags are best practice.

**Fix:** Add Twitter Card tags. Most frameworks allow setting both OG and Twitter tags in the same metadata configuration. Use `summary_large_image` for pages with a hero image, `summary` for others.

**Affects:** SEO (indirect)

### Viewport meta for mobile

**What it means:** The `<meta name="viewport" content="width=device-width, initial-scale=1">` tag tells mobile browsers how to scale the page. Without it, mobile devices render the page at desktop width and shrink it down.

**How to check in code:** Search for `viewport` in the root layout or HTML template. It should be present exactly once.

**Good:** Viewport meta present with `width=device-width, initial-scale=1`. No `maximum-scale=1` or `user-scalable=no` (these prevent zooming, which is an accessibility issue).

**Bad:** No viewport meta (page renders as desktop on mobile). `user-scalable=no` preventing zoom. Multiple conflicting viewport tags.

**Fix:** Add the viewport meta to the root layout: `<meta name="viewport" content="width=device-width, initial-scale=1">`. Remove any zoom-preventing attributes.

**Affects:** SEO (mobile-friendliness is a ranking factor) + AEO (indirect — mobile-friendly pages are more fully indexed)

---

## 3. Heading Structure

Does the heading hierarchy communicate page structure clearly to both humans and machines?

### Single H1 per page

**What it means:** Each page should have exactly one H1 that states the page's primary topic. Multiple H1s dilute the signal about what the page is about.

**How to check in code:** Search for `<h1` or heading component usage in page templates. Check: does each page have exactly one H1? Is it the most prominent heading? Does it match the page's primary keyword/topic? Is it distinct from the `<title>` tag (though related)?

**Good:** One H1 per page that clearly states the topic. Matches search intent for the target query. Contains the primary keyword naturally.

**Bad:** Multiple H1s (common when a component library uses H1 for styling rather than semantics). No H1 at all. H1 is a generic greeting ("Welcome!") rather than a topic statement.

**Fix:** Ensure each page template renders exactly one H1. If component libraries are using H1 for visual weight, change them to use CSS for sizing and the correct semantic heading level.

**Affects:** SEO + AEO (AI uses H1 to identify the page's primary topic)

### Logical H2-H6 hierarchy

**What it means:** Headings should nest logically: H2s under H1, H3s under H2s, without skipping levels. The heading structure should read like a table of contents that communicates the page's structure without any body text.

**How to check in code:** Map the heading hierarchy of key pages. Look for: H3 appearing without a preceding H2 (skipped level), headings used for visual styling rather than semantic structure, inconsistent heading levels across similar page types.

**Good:** Headings form a logical outline. Reading only the headings gives you a clear understanding of what the page covers. No skipped levels.

**Bad:** Headings used for styling (H3 because "it's the right size"). Skipped levels (H1 followed by H4). Flat hierarchy (everything is H2, no H3s for subtopics).

**Fix:** Restructure headings to form a logical outline. Use CSS for visual styling, HTML heading levels for semantic structure. Create a heading convention for the project (e.g., "page title is H1, section headings are H2, subsections are H3").

**Affects:** SEO + AEO (AI navigates heading hierarchy to find relevant sections for specific queries)

### Heading text matches search intent

**What it means:** Headings should contain the words and phrases that searchers use when looking for this content. Not keyword-stuffed, but naturally phrased to match how people search.

**How to check in code:** Read the headings on key pages. Do they use the same language a searcher would use? For informational content, are headings phrased as questions the audience asks? For product pages, do headings name the features/benefits users search for?

**Good:** Headings use natural language that matches search queries. Informational headings are phrased as questions ("How do I add structured data?" not "Structured Data Implementation Protocol"). Product headings name specific benefits.

**Bad:** Headings are clever but not searchable ("The Magic Touch" instead of "One-Click Deployment"). Headings are internal jargon ("Phase 2 Features"). Headings are too vague ("More Information").

**Fix:** Rewrite headings to match the language your audience uses in search. Use question phrasing for informational content. Keep headings specific and descriptive.

**Affects:** SEO (keyword relevance) + AEO (AI matches queries to heading text to find relevant sections)

---

## 4. Structured Data

Does the site provide machine-readable metadata that search engines and AI systems can parse directly?

### Organization schema on homepage

**What it means:** A JSON-LD block on the homepage that tells search engines and AI what the organization is — name, URL, logo, social profiles, contact information. This is the foundation for entity establishment.

**How to check in code:** Search for `"@type": "Organization"` or `schema.org/Organization` in the homepage template or layout. Check: are name, URL, and logo included? Are social profile URLs (sameAs) listed? Is contact information present?

**Good:**
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Company Name",
  "url": "https://example.com",
  "logo": "https://example.com/logo.png",
  "sameAs": [
    "https://twitter.com/company",
    "https://linkedin.com/company/company",
    "https://github.com/company"
  ],
  "contactPoint": {
    "@type": "ContactPoint",
    "email": "support@example.com",
    "contactType": "customer support"
  }
}
```

**Bad:** No Organization schema. Organization schema with only name and URL (missed opportunity). Organization schema on every page instead of just the homepage (not harmful but unnecessary).

**Fix:** Add a JSON-LD block to the homepage layout. Include all available fields: name, URL, logo, social profiles, contact information, founding date, description.

**Affects:** SEO (enables knowledge panel) + AEO (establishes entity identity for AI systems)

### Article/BlogPosting on content pages

**What it means:** JSON-LD markup on blog posts and articles that identifies the content type, author, publish date, modified date, and description. Enables rich results and helps AI systems understand content provenance.

**How to check in code:** Search for `"@type": "Article"` or `"@type": "BlogPosting"` in blog/content templates. Check: are `headline`, `author`, `datePublished`, `dateModified`, `description`, and `image` included? Is the author a Person or Organization with a name and URL?

**Good:**
```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "How to Implement Structured Data",
  "author": {
    "@type": "Person",
    "name": "Jane Smith",
    "url": "https://example.com/team/jane"
  },
  "datePublished": "2025-01-15",
  "dateModified": "2025-03-20",
  "description": "A step-by-step guide to adding JSON-LD structured data to your website.",
  "publisher": {
    "@type": "Organization",
    "name": "Company Name",
    "logo": { "@type": "ImageObject", "url": "https://example.com/logo.png" }
  }
}
```

**Bad:** No Article schema on blog posts. Missing author (anonymous content is less citable). Missing dates (AI systems can't assess freshness). `dateModified` always equals `datePublished` (useless).

**Fix:** Add Article/BlogPosting schema to blog post templates. Populate from CMS fields or frontmatter. Ensure `dateModified` is updated when content is revised.

**Affects:** SEO (rich results) + AEO (authorship and freshness signals for AI citation)

### Product schema on product pages

**What it means:** JSON-LD markup on product pages that describes the product name, description, price, availability, reviews, and images. Enables rich product results in search.

**How to check in code:** Search for `"@type": "Product"` in product page templates. Check: are `name`, `description`, `image`, `offers` (with `price`, `priceCurrency`, `availability`), and `aggregateRating` included?

**Good:** Complete Product schema with offers, reviews, and images. Price and availability are accurate and match the visible page content.

**Bad:** No Product schema. Product schema with mismatched prices (structured data says one price, page shows another — this is a policy violation). Missing availability status.

**Fix:** Add Product schema to product page templates. Populate from product data. Ensure structured data always matches visible content.

**Affects:** SEO (rich product results) + AEO (AI extracts product information from structured data)

### FAQPage schema on FAQ content

**What it means:** JSON-LD markup that identifies FAQ content — question-and-answer pairs that search engines display as expandable results and AI systems extract directly.

**How to check in code:** Search for `"@type": "FAQPage"` in FAQ pages or sections. Check: does each question-answer pair use `Question` and `acceptedAnswer` types? Do the questions and answers match the visible content?

**Good:**
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is answer engine optimization?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Answer engine optimization (AEO) is the practice of structuring content so AI-powered search systems can extract, summarize, and cite it."
      }
    }
  ]
}
```

**Bad:** FAQ content exists on the page but has no FAQPage schema. Schema questions don't match the visible questions. Answers in schema are different from answers on the page (policy violation).

**Fix:** Add FAQPage schema to any page with FAQ content. Generate from the actual FAQ content on the page. Keep schema and visible content in sync.

**Affects:** SEO (FAQ rich results) + AEO (AI extracts FAQ schema directly as answers)

### HowTo schema on tutorials

**What it means:** JSON-LD markup that identifies step-by-step content — tutorials, guides, and instructions. Enables rich how-to results in search and provides structured steps for AI extraction.

**How to check in code:** Search for `"@type": "HowTo"` in tutorial or guide templates. Check: does each step use the `HowToStep` type with `name` and `text`? Are `totalTime`, `estimatedCost`, and `tool`/`supply` included where relevant?

**Good:**
```json
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "How to Add Structured Data to Your Site",
  "totalTime": "PT30M",
  "step": [
    {
      "@type": "HowToStep",
      "name": "Choose your schema type",
      "text": "Identify which schema.org type matches your content..."
    }
  ]
}
```

**Bad:** Tutorial content without HowTo schema. Steps not in `HowToStep` format. Missing time estimate when the information is available.

**Fix:** Add HowTo schema to tutorial and guide content. Break content into discrete steps. Include time estimates and tool/supply lists where applicable.

**Affects:** SEO (rich how-to results) + AEO (AI extracts structured steps for how-to queries)

### BreadcrumbList for navigation

**What it means:** JSON-LD markup that describes the page's position in the site hierarchy. Enables breadcrumb display in search results and helps AI understand site structure.

**How to check in code:** Search for `"@type": "BreadcrumbList"` in layout or navigation components. Check: does the breadcrumb trail accurately reflect the page's location in the site hierarchy? Are all items linked except the current page?

**Good:** BreadcrumbList schema on every page that reflects the navigation hierarchy. Items have `name` and `item` (URL) properties. Matches the visible breadcrumb navigation.

**Bad:** No breadcrumb schema. Breadcrumb schema doesn't match the visible navigation. Only present on some pages.

**Fix:** Add BreadcrumbList schema to the layout template. Generate from the page's URL path or navigation configuration.

**Affects:** SEO (breadcrumb rich results, site hierarchy understanding) + AEO (helps AI understand content organization)

### Review/AggregateRating where applicable

**What it means:** JSON-LD markup for user reviews and aggregate ratings. Enables star ratings in search results — one of the most eye-catching rich result types.

**How to check in code:** Search for `"@type": "Review"` or `"@type": "AggregateRating"` in product or service page templates. Check: are ratings and reviews from real users? Do they match visible reviews on the page?

**Good:** AggregateRating on product/service pages with real review data. Individual Review schema for detailed reviews. Rating values match visible stars/scores.

**Bad:** Fake reviews in structured data. Ratings in schema that don't match the page. Self-reviews (reviewing your own product — against Google's policies).

**Fix:** Add AggregateRating schema to pages that display real user reviews. Populate from actual review data. Never fabricate or inflate ratings in structured data.

**Affects:** SEO (star rating rich results) + AEO (AI uses ratings for product comparisons)

### Validating structured data

**What it means:** Structured data should be validated to ensure it's syntactically correct and follows schema.org specifications.

**How to verify:** Use Google's Rich Results Test patterns — the JSON-LD should be valid JSON, use correct `@type` values, include all required properties for the type, and not contain values that contradict visible page content.

**Common validation issues:** Missing required fields (Article without `author`), incorrect date formats (should be ISO 8601: `YYYY-MM-DD`), URLs that 404, `@type` values that aren't valid schema.org types, nested objects missing their `@type`.

**Affects:** SEO + AEO

---

## 5. Performance

Does the site load fast enough to rank well and provide a good user experience?

### Core Web Vitals targets

**What it means:** LCP under 2.5s, INP under 200ms, CLS under 0.1. These are Google's performance metrics and a confirmed ranking signal.

**How to check in code:** Look for large unoptimized images (LCP culprits), render-blocking scripts (LCP and INP), layout shifts from dynamically loaded content (CLS). Check for: images without explicit `width` and `height`, fonts without `font-display: swap`, JavaScript that blocks rendering.

**Good:** Images are optimized (WebP/AVIF, proper sizing, lazy loading for below-fold). Fonts use `font-display: swap` or `optional`. Critical CSS is inlined. JavaScript is deferred.

**Bad:** Large PNG/JPG hero images (slow LCP). Web fonts that block rendering (FOIT). Dynamically injected content that pushes the page around (CLS). Heavy JavaScript that blocks interaction (INP).

**Fix:** Optimize images (next/image, responsive srcset, WebP/AVIF). Preload critical fonts. Defer non-critical JavaScript. Add `width` and `height` to images and embeds.

**Affects:** SEO (ranking signal) + AEO (indirect — faster pages are more fully crawled)

### Image optimization

**What it means:** Images use modern formats, appropriate sizes, and lazy loading for below-fold content.

**How to check in code:** Search for `<img` tags. Check: format (WebP/AVIF vs. PNG/JPG), `srcset` for responsive images, `loading="lazy"` for below-fold images, explicit `width` and `height` attributes, `alt` text (covered in heading/content section but also technical).

**Good:** Modern formats (WebP with JPEG fallback), responsive `srcset`, lazy loading for below-fold, explicit dimensions, descriptive alt text.

**Bad:** Large PNG/JPG files, no responsive images, all images loaded eagerly, no dimensions (causes layout shifts), missing alt text.

**Fix:** Convert to WebP/AVIF. Add `srcset` with appropriate breakpoints. Add `loading="lazy"` to below-fold images. Set explicit `width` and `height`. Use framework image components (next/image, Astro Image) that handle this automatically.

**Affects:** SEO (page speed, image search) + AEO (alt text helps AI understand image content)

### Code splitting and bundle size

**What it means:** JavaScript should be split so users only download code needed for the current page, not the entire application.

**How to check in code:** Check bundle analyzer output or build output. Look for: single large JavaScript bundles, vendor libraries included in the main bundle, dynamic imports for routes and heavy components.

**Good:** Route-based code splitting. Vendor code in a separate chunk. Heavy libraries (charts, editors, maps) loaded on demand. Total JS under 200KB for initial page load.

**Bad:** Single bundle over 500KB. Entire application loaded on first page view. Heavy libraries imported at the top level.

**Fix:** Implement route-based code splitting (most frameworks do this by default). Dynamic import heavy components. Move vendor code to a separate chunk. Analyze and eliminate unused dependencies.

**Affects:** SEO (page speed)

### Font loading strategy

**What it means:** Web fonts should load without blocking rendering or causing layout shifts.

**How to check in code:** Search for `@font-face` declarations and font loading configuration. Check for: `font-display` property, font preloading, font subsetting, number of font files loaded.

**Good:** `font-display: swap` or `optional`. Critical fonts preloaded (`<link rel="preload" as="font">`). Font files subsetted to include only needed characters. No more than 2-3 font files for initial page load.

**Bad:** No `font-display` (blocks rendering until font loads). Loading 10+ font weights/styles. No preloading. Full font files when only Latin characters are needed.

**Fix:** Add `font-display: swap` (or `optional` for non-critical fonts). Preload critical fonts. Subset fonts to needed character sets. Limit to 2-3 font files for initial load.

**Affects:** SEO (LCP, CLS)

---

## 6. Mobile

Does the site work well on mobile devices?

### Responsive design

**What it means:** The site adapts to different screen widths using CSS media queries, flexible grids, and responsive images.

**How to check in code:** Search for media queries (`@media`), responsive utility classes (Tailwind's `sm:`, `md:`, `lg:`), or CSS container queries. Check for: fixed-width layouts, horizontal overflow, images that extend beyond the viewport.

**Good:** Fluid layout that adapts to all screen sizes. No horizontal scrolling. Content readable without zooming. Navigation adapts (hamburger menu or equivalent on small screens).

**Bad:** Fixed-width layout that requires horizontal scrolling. Text too small to read on mobile. Touch targets smaller than 48x48px. Content hidden on mobile that's visible on desktop (content parity issue).

**Fix:** Use responsive design patterns (fluid grids, media queries, responsive images). Ensure content parity between mobile and desktop. Test at common breakpoints (375px, 768px, 1024px, 1440px).

**Affects:** SEO (mobile-first indexing — Google indexes the mobile version) + AEO (indirect)

### Tap targets adequately sized

**What it means:** Interactive elements (buttons, links, form inputs) must be large enough to tap accurately on touch devices. Google's minimum: 48x48px with adequate spacing between targets.

**How to check in code:** Check button and link sizing. Look for: small text links with no padding, closely spaced interactive elements, form inputs without adequate height.

**Good:** All interactive elements are at least 48x48px (including padding). At least 8px spacing between adjacent tap targets.

**Bad:** Small text links as the primary navigation. Closely spaced icon buttons. Form inputs under 44px height.

**Fix:** Add padding to increase tap target size. Space adjacent interactive elements. Use minimum height on form inputs.

**Affects:** SEO (mobile usability)

### Content parity between mobile and desktop

**What it means:** The mobile version of the site should have the same content as the desktop version. Google uses mobile-first indexing — if content is hidden on mobile, it may not be indexed.

**How to check in code:** Search for responsive hiding utilities (`hidden`, `display: none`, `visibility: hidden`) applied at mobile breakpoints. Check if these hide decorative elements (acceptable) or content (problematic).

**Good:** All content is accessible on mobile. Layout adapts but content is preserved. Accordions or tabs may be used to organize mobile content as long as the content is in the DOM and accessible.

**Bad:** Entire sections hidden on mobile with `display: none`. Product descriptions truncated on mobile. FAQ sections removed on mobile.

**Fix:** Reorganize mobile layout instead of hiding content. Use accordions or progressive disclosure instead of `display: none`. Ensure all indexable content is present in the mobile DOM.

**Affects:** SEO (mobile-first indexing) + AEO (AI crawlers may see the mobile version)

---

## 7. Internal Linking

Does the link structure help crawlers discover content and distribute ranking authority?

### Logical link structure

**What it means:** Pages are connected through a logical hierarchy of links that reflects the site's content structure. Important pages receive more internal links.

**How to check in code:** Map the link structure from the homepage. Check: how many clicks to reach important content? Are there orphan pages (pages with no internal links pointing to them)? Does the navigation link to the most important pages?

**Good:** Important pages are linked from the navigation or homepage. Content pages link to related content. There's a clear hierarchy from homepage to category pages to detail pages.

**Bad:** Important pages only accessible through deep navigation. Orphan pages that can only be reached from the sitemap. Flat link structure where everything links to everything (dilutes link value).

**Fix:** Add navigation links to important pages. Add contextual links between related content. Create hub pages that link to all content within a topic cluster.

**Affects:** SEO (crawl efficiency, link equity distribution) + AEO (helps AI understand content relationships)

### Important pages reachable within 3 clicks

**What it means:** Any important page should be reachable from the homepage in 3 clicks or fewer. The deeper a page is, the less crawl priority and link equity it receives.

**How to check in code:** Map the click depth of key pages. Navigation items are 1 click. Pages linked from navigation pages are 2 clicks. And so on.

**Good:** All important content within 3 clicks. Key conversion pages (pricing, signup) within 1-2 clicks. Blog posts within 2 clicks (homepage -> blog -> post).

**Bad:** Important content buried 4+ clicks deep. Pricing page accessible only through a submenu of a submenu.

**Fix:** Restructure navigation to surface important pages. Add direct links from the homepage or key landing pages to high-priority content.

**Affects:** SEO (crawl depth, link equity)

### Descriptive anchor text

**What it means:** Link text should describe what the linked page is about, not use generic text like "click here" or "read more."

**How to check in code:** Search for common generic anchor patterns: `>click here<`, `>read more<`, `>learn more<`, `>here<`. Check navigation link text — is it descriptive?

**Good:** "See our pricing plans" linking to pricing. "How to implement structured data" linking to a tutorial. Anchor text that would make sense out of context.

**Bad:** "Click here to see our pricing plans" with "click here" as the link. "Read more" as the only link text. URLs as anchor text.

**Fix:** Rewrite anchor text to describe the destination page's content. Include relevant keywords naturally. Remove "click here" patterns entirely.

**Affects:** SEO (anchor text is a relevance signal) + AEO (helps AI understand page relationships)

### Breadcrumb navigation

**What it means:** A secondary navigation showing the user's location in the site hierarchy (Home > Category > Subcategory > Page). Helps users orient and provides additional internal links with descriptive anchor text.

**How to check in code:** Search for breadcrumb components or markup. Check: does it reflect the actual site hierarchy? Is it present on all content pages? Does it have corresponding BreadcrumbList structured data?

**Good:** Breadcrumbs on all content pages. Reflects the real hierarchy. Each level is linked. Accompanied by BreadcrumbList schema.

**Bad:** No breadcrumbs. Breadcrumbs that don't match the URL structure. Breadcrumbs only on some pages.

**Fix:** Add a breadcrumb component to content page templates. Generate from the URL path or navigation configuration. Add BreadcrumbList schema to match.

**Affects:** SEO (navigation, structured data) + AEO (helps AI understand content hierarchy)
