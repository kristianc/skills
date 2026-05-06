# AXES.md

Differentiation axes — the dimensions customers use to evaluate alternatives. For each axis: what it means concretely, how to evaluate where a product falls, what "winning" looks like, what it costs, and examples of products that win on it.

## How axes work

An axis is a dimension of evaluation, not a feature. "Has dark mode" is a feature. "Customizability" is an axis. Customers evaluate products across multiple axes simultaneously, but they weight them unevenly — and the weights vary by segment.

The skill's job is to identify which axes matter most to the target customer and where the product has a durable advantage. Not every axis below is relevant to every category. Some categories have axes unique to them that aren't on this list. Use this as a starting inventory, then add category-specific axes during the mapping step.

## Common axes

### Price

**What it means:** Total cost to the customer over the relevant time horizon — not just sticker price but implementation cost, migration cost, training cost, and ongoing operational cost. A product with a low sticker price and high implementation cost is not winning on price.

**How to evaluate:** Calculate total cost of ownership (TCO) for the target customer profile over 12 months. Include all line items: subscription, overage charges, required add-ons, implementation services, internal engineering time for setup and maintenance.

**What winning looks like:** Lowest TCO for the target customer's usage pattern. Not necessarily the lowest sticker price — the product with the lowest sticker price and highest hidden costs loses on price to the one with a higher sticker price and no surprises.

**What it costs to win:** Margin. Winning on price usually means lower margins, which means less to spend on sales, support, and R&D. Products that win on price often compensate with volume or with a self-serve motion that keeps sales costs low.

**Examples:** Hetzner wins on price in cloud hosting by accepting lower margins and running their own data centers. Linear doesn't try to win on price — they charge more than free alternatives and compete on other axes.

**Trap:** Competing on price against a competitor with fundamentally lower cost structure (larger scale, vertical integration, different business model) is a losing strategy. You can't out-price someone who can always go lower.

### Speed / performance

**What it means:** How fast the product performs its core job. Latency, throughput, time-to-result. Varies by category: for a database, it's query latency; for a CI tool, it's build time; for an analytics platform, it's time from question to answer.

**How to evaluate:** Identify the core operation the customer cares about and measure it. Look at published benchmarks (with skepticism), user-reported performance in reviews, and documentation on architecture. If the product can be trialed, measure directly.

**What winning looks like:** Measurably faster on the operations the target customer performs most. Not faster on synthetic benchmarks that don't match real workloads.

**What it costs to win:** Engineering investment in optimization, often at the expense of feature breadth. Products that win on speed typically make architectural decisions early (columnar storage, edge deployment, compiled languages) that constrain what else they can build easily.

**Examples:** ClickHouse wins on analytical query speed through columnar architecture. Figma won on collaboration speed by building a browser-native renderer instead of wrapping a desktop app.

**Trap:** Speed claims that can't be reproduced at the customer's scale. "Fast on our benchmark" is not the same as "fast on your data."

### Ease of setup

**What it means:** Time and effort from "I want to evaluate this" to "I'm getting value." Includes signup, configuration, data migration, integration with existing tools, and time to first meaningful use.

**How to evaluate:** Attempt the setup. Measure wall-clock time from signup to first real use. Count the number of decisions the user must make, the number of external dependencies, and the number of times they need to leave the product to consult documentation.

**What winning looks like:** The target customer gets to value in minutes or hours, not days or weeks. Zero-config defaults that work for the common case. Guided onboarding that doesn't require reading docs.

**What it costs to win:** Flexibility. Products that are easy to set up make opinionated choices about defaults, which means they work less well for customers whose needs diverge from the defaults. Ease of setup and depth of customization are in tension.

**Examples:** Vercel wins on ease of deployment for frontend apps — `git push` and it's live. Stripe won on ease of payment integration — a few lines of code vs. months of bank integration.

**Trap:** Confusing "easy to sign up" with "easy to set up." A slick signup flow followed by a week of configuration is not winning on ease of setup.

### Depth of functionality

**What it means:** How much of the customer's problem the product solves, and how thoroughly. The depth axis favors products that handle edge cases, advanced use cases, and complex workflows that simpler tools can't.

**How to evaluate:** Map the customer's full workflow. Identify which steps the product handles natively, which require workarounds, and which require leaving the product entirely. Count the workarounds.

**What winning looks like:** The target customer can accomplish their entire workflow — including the messy parts — without leaving the product or building workarounds. Advanced users find features that match their sophistication.

**What it costs to win:** Simplicity. Deep products are harder to learn, harder to set up, and harder to maintain. They intimidate users who don't need the depth. Winning on depth means losing some customers who wanted something simpler.

**Examples:** Excel wins on depth for spreadsheet use cases — there is almost nothing you can't make it do. Salesforce wins on depth for CRM — it handles enterprise sales workflows that simpler CRMs can't.

**Trap:** Depth without discoverability. A product with 500 features that users can't find is not winning on depth — it's losing on ease of use while paying the cost of depth.

### Integration breadth

**What it means:** How well the product connects with the customer's existing tool stack. Number of native integrations, quality of APIs, webhook support, ecosystem of third-party connectors.

**How to evaluate:** List the target customer's existing tools. Check which ones the product integrates with natively. Test the integrations — do they actually work, or are they listed but broken? Check the API documentation for completeness and developer experience.

**What winning looks like:** The product slots into the customer's existing stack without requiring them to change other tools or build custom glue code. Data flows bidirectionally where it should.

**What it costs to win:** Maintenance burden. Every integration is an ongoing commitment — APIs change, partners deprecate features, customers expect integrations to stay current. Winning on integrations means a permanent tax on engineering.

**Examples:** Zapier wins on integration breadth as a platform. Slack wins on integrations within team communication by having a massive app directory and well-documented API.

**Trap:** Counting integrations without evaluating depth. An integration that syncs names but not custom fields is not the same as one that provides full bidirectional sync. Customers learn this during evaluation, not from the marketing page.

### Support quality

**What it means:** How well the company helps customers when things go wrong or when they need guidance. Response time, resolution quality, channel availability, and whether support understands the customer's actual problem.

**How to evaluate:** Look at published SLAs, support tier structure, community forums (does the company respond?), and review sites for support-related praise or complaints. Note whether support is gated by tier — many products reserve quality support for enterprise customers.

**What winning looks like:** The target customer gets fast, knowledgeable help through a channel they prefer. Issues are resolved, not deflected. Support engineers understand the product deeply enough to solve non-obvious problems.

**What it costs to win:** Headcount and hiring quality. Good support requires people who understand both the product and the customer's context. This is expensive, doesn't scale linearly, and competes with engineering for technical talent.

**Examples:** Rackspace historically won on support in hosting ("Fanatical Support"). Pagerduty offers SLAs on response times for incidents.

**Trap:** Claiming great support without the unit economics to sustain it. A startup with 3 support engineers can deliver exceptional support at 50 customers. At 5,000, the same team delivers terrible support. Position on support only if the business model supports it at scale.

### Data ownership / privacy

**What it means:** Whether the customer controls their data — where it lives, who can access it, whether they can export it, whether it's used to train models, and what regulatory frameworks the product complies with.

**How to evaluate:** Read the terms of service, privacy policy, and data processing agreements. Check for certifications (SOC 2, ISO 27001, GDPR compliance). Check whether data can be self-hosted or runs only in the vendor's cloud. Check export capabilities — can the customer get their data out in a standard format?

**What winning looks like:** The customer's data stays under their control — either self-hosted or in a cloud deployment where the vendor's access is auditable and minimal. Data is exportable in standard formats. Compliance certifications match the customer's regulatory requirements.

**What it costs to win:** Revenue from data. Products that win on data ownership can't monetize customer data, can't use it for model training (in AI products), and often can't offer certain features that require cross-customer data aggregation. Self-hosted options cost more to support.

**Examples:** Plausible Analytics wins on privacy in web analytics by being the anti-Google-Analytics — no cookies, GDPR-compliant by design, self-hostable. GitLab wins on data ownership by offering self-managed deployment.

**Trap:** Privacy as checkbox compliance rather than architectural commitment. Adding a "GDPR compliant" badge while still collecting telemetry doesn't win on this axis — it loses trust when discovered.

### Customizability

**What it means:** How much the customer can adapt the product to their specific workflow, branding, processes, or requirements. Ranges from configuration (toggles and settings) to extensibility (APIs and plugins) to full programmability (scripting, custom code).

**How to evaluate:** Identify the target customer's need for customization. Check what's configurable without code, what's extensible via API, and what requires the vendor's professional services. Test whether customizations survive product updates.

**What winning looks like:** The target customer can make the product work the way their business works, without waiting for the vendor to build features. Customizations are first-class, not fragile workarounds.

**What it costs to win:** Consistency and simplicity. Highly customizable products are harder to support (every customer's instance is different), harder to update (customizations can break), and harder to onboard (more decisions to make). Winning on customizability means losing some appeal for customers who want opinionated defaults.

**Examples:** Notion wins on customizability in workspace tools — blocks, databases, templates make it adaptable to almost any workflow. Shopify wins on customizability in e-commerce through its theme and app ecosystem.

**Trap:** Customizability that requires the vendor's professional services. If the customer can't customize it themselves, it's not a customizability advantage — it's a services revenue stream.

### Trust / brand / longevity

**What it means:** The customer's confidence that the product will exist, be maintained, and be reliable in 2–5 years. Driven by company size, funding, profitability, track record, security posture, and reputation.

**How to evaluate:** Company age, revenue trajectory (if public), funding history, customer base (logos, case studies), security certifications, uptime history, and public incidents. For startups: burn rate signals, team stability, product velocity as a proxy for health.

**What winning looks like:** The target customer believes the product is a safe bet — that choosing it won't result in a painful migration in 18 months because the company folded or pivoted. For regulated industries, certifications and audit trails that satisfy compliance teams.

**What it costs to win:** Time. Trust is earned slowly and lost quickly. Startups can't compete on trust against established incumbents through marketing — they compete on it by shipping consistently, being transparent about incidents, and building a public track record.

**Examples:** AWS wins on trust in cloud infrastructure — no one ever got fired for choosing AWS. Salesforce wins on trust in CRM for the same reason. Both products have meaningful weaknesses on other axes, but trust compensates.

**Trap:** Conflating brand awareness with trust. A well-known product that ships unreliably does not win on trust. A lesser-known product with a spotless uptime record and transparent incident communication can.

## Identifying category-specific axes

The axes above are common across most B2B categories. But every category has axes unique to it. To find them:

1. **Read recent reviews (last 12 months) on G2 or Capterra for the category.** Look at what customers praise and complain about. The recurring themes that don't map to the common axes above are your category-specific axes.
2. **Read comparison threads on Reddit and Hacker News.** When real users compare products, the dimensions they use reveal the axes they evaluate on.
3. **Look at what competitors' comparison pages emphasize.** Competitors have already done axis-identification work — their comparison pages reveal which axes they think they win on.
4. **Ask the user.** What do prospects ask about in sales calls? What do customers cite as the reason they chose the product? What do churned customers cite as the reason they left?

## Stated vs. revealed axes

Customers often say they evaluate on one axis but actually evaluate on another. This is not dishonesty — it's the difference between what they think matters and what drives their actual decision.

**How to detect stated vs. revealed gaps:**

- **Stated: "Data ownership is critical for us." Revealed: they chose the cheapest option with no data export.** Look at what the customer actually bought, not what they said mattered in the evaluation.
- **Stated: "We need deep customization." Revealed: they use the product with default settings.** Feature usage data (when available) reveals which capabilities customers actually use.
- **Stated: "Performance is our top priority." Revealed: they chose the product with the best onboarding.** Time-to-value often beats raw performance in actual decisions, even when buyers rank performance higher in surveys.

**How to use this in positioning:**

Position on revealed axes, not stated ones. If customers say they evaluate on depth but actually choose on ease of setup, position on ease of setup — but frame it in terms of depth ("deep enough for real work, without the setup cost of tools built for power users"). This respects both their stated values and their actual decision criteria.
