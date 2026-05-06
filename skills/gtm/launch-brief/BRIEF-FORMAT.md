# BRIEF-FORMAT.md

The launch brief is the deliverable. It is one page. Everything the team needs to execute the launch is on that page — and anything that does not fit is either not sharp enough or not part of this launch.

This file covers the template, guidance on each section, and the reasoning behind the format.

## Why one page

A one-page constraint is not about brevity for its own sake. It is a thinking tool.

If the audience definition takes a paragraph, the audience is too broad. If the claim needs three sentences, it is actually two claims. If the channel plan has eight rows, the team will execute none of them well.

The one-page constraint forces compression, and compression forces clarity. When the brief is too long, the correct response is not "use a smaller font" — it is "sharpen the thinking."

If genuinely too much must be said — a multi-product launch, a launch that serves two distinct audiences — the answer is two briefs, not one longer brief. Each brief gets its own audience, claim, and channel plan. They can share a launch date and an entry point, but the strategic documents are separate.

## The template

```
Launch: [one-line description]

Date: [target date]

Primary audience: [who, specifically]

The claim: [one sentence — what changed and why they should care]

Proof points:
1. [specific, verifiable]
2. [specific, verifiable]
3. [specific, verifiable]

Narrative arc: [problem→solution / before→after / limitation→capability]

Entry point: [the canonical URL or asset]

Channel plan:
| Channel | Format | Audience segment | Key message angle | Timing |
|---------|--------|-----------------|-------------------|--------|
| ...     | ...    | ...             | ...               | ...    |

What we are NOT saying: [messages that are tempting but wrong]

Success signal: [how we will know this landed]
```

## Section-by-section guidance

### Launch

One line. What is being launched, stated plainly.

**Good:** "Pipeline Monitor: real-time error detection for data pipelines."
**Bad:** "Exciting new feature that helps data teams work more efficiently." (This describes nothing. Which feature? What does it do? "Exciting" is the writer's opinion, not information.)

**Common mistake:** Cramming the value proposition into the launch line. The launch line is factual — what it is. The claim is where the value lives.

### Date

The target date for the public announcement. If the date is not set, write "TBD" and note the dependency. A brief without a date can still be useful for message alignment, but it is not actionable until the date is set.

If the launch is time-sensitive — pegged to a conference, a competitor move, or a seasonal window — note why the date matters. This helps the team prioritize when tradeoffs arise.

### Primary audience

One to two sentences maximum. This is the output of the audience grilling (AUDIENCE-GRILLING.md). It should be specific enough that someone unfamiliar with the product could identify members of this audience.

**Good:** "Python developers who currently use pandas for data pipelines over 1M rows and have hit performance limits."
**Bad:** "Data engineers." (Too broad. Which data engineers? In what situation? With what problem?)
**Bad:** "Our power users." (Meaningless to anyone outside the company. What makes someone a power user? What do they need?)

**Common mistake:** Listing multiple audiences as co-primary. If the brief says "primary audience: developers and enterprise buyers," it is two launches pretending to be one. Pick one. The other is secondary at best.

### The claim

One sentence. The strategic core of the brief. Everything else derives from this.

The claim has two parts: what changed (the product fact) and why they should care (the audience connection). See MESSAGE-TESTING.md for the full framework.

**Good:** "Query results in under 2 seconds on datasets up to 50M rows — so you stop waiting and start exploring."
**Bad:** "Powerful new query engine with industry-leading performance." (No product fact — how powerful? No audience connection — leading for whom? Both halves are adjectives pretending to be information.)

**Common mistake:** Writing the claim as a tagline. The claim is not copy — it is strategy. It should be plain, precise, and boring to everyone except the target audience. If the claim sounds like an ad, it is probably hiding imprecision behind polish.

### Proof points

Two to three specific, verifiable facts that back the claim.

**Good:** "1. Sub-2-second query performance on 50M row datasets (benchmarked on standard hardware). 2. 10x improvement over previous engine on the same workloads. 3. Zero config migration — existing queries work without changes."
**Bad:** "1. Best-in-class performance. 2. Seamless experience. 3. Enterprise-ready." (None of these can be checked. They are adjectives, not proof.)

**Common mistake:** Listing features instead of proof. "Supports batch processing" is a feature. "Processes 1M records in under 60 seconds" is proof. Features describe what the product does; proof demonstrates that the claim is true.

**Common mistake:** Including proof points that serve a different audience than the one named in the brief. If the primary audience is developers, a proof point about "reduced total cost of ownership" serves the buyer, not the developer. Save it for the buyer's brief.

### Narrative arc

One of three: problem-to-solution, before-to-after, or limitation-to-capability. State which one. See MESSAGE-TESTING.md for when each fits.

**Good:** "Before→after: Data pipeline debugging used to mean sifting through logs hours after failure. Now errors surface in real time with the failing step identified."
**Bad:** "We tell the story of how we built this and why it matters." (This is not a narrative arc. It is a description of a blog post structure, and a self-centered one at that.)

**Common mistake:** Not choosing. Writing "we will use a mix of narratives" means no narrative has been chosen, and the copy will wander. Pick one. The other arcs can inform secondary content on specific channels, but the brief has one arc.

### Entry point

The canonical asset that everything else points back to. Usually a blog post, a landing page, or a changelog entry. Everything in the channel plan — the tweet, the community post, the email — links back to this.

**Good:** "Blog post at /blog/pipeline-monitor — detailed walkthrough with demo GIF and benchmark table."
**Bad:** "Our website." (Which page? What does it contain? How does it support the claim?)

**Common mistake:** No entry point. Channel-specific content without a canonical source creates fragmented messaging. The entry point is the single source of truth that the team can keep accurate; channel posts can simplify or adapt, but they point back.

### Channel plan

A table with 2-4 rows. See CHANNEL-SELECTION.md for the selection framework.

**Common mistake:** Too many channels. If the table has more than 4 rows, the team will either execute poorly on all of them or quietly drop most. Three channels done well create more impact than eight done generically.

**Common mistake:** The channel plan is all owned channels (blog, email, in-app). These are distribution, not discovery. At least one channel should be where the audience already goes — a community, a forum, a newsletter they read.

### What we are NOT saying

Messages that are tempting but wrong — off-brand, unverifiable, aimed at the wrong audience, or true but distracting from the primary claim.

This section prevents message drift during execution. Without it, the person writing the tweet adds a claim the brief does not support, the email marketer mentions a use case aimed at a different audience, and the launch says six things instead of one.

**Good:** "We are not saying this is an 'AI-powered' feature — the underlying model is not the point, the speed is. We are not comparing to [Competitor] — the claim stands on its own. We are not mentioning the enterprise tier — this launch is for individual developers."
**Bad:** Leaving this section empty. Every launch has tempting-but-wrong messages. If the team cannot name them, they have not thought about message discipline.

### Success signal

How the team will know the launch landed — not vanity metrics, but signals that the target audience received and understood the message.

**Good:** "Within one week: 50+ new Pipeline Monitor activations from users who were not in the beta. Qualitative: at least 3 organic mentions in the data engineering subreddit or community Slack."
**Bad:** "Increased traffic to the website." (Traffic from whom? To which page? Traffic is not a signal that the message landed with the right audience — it is a signal that something generated clicks.)
**Bad:** "Revenue growth in Q3." (Too distant, too many confounders. The success signal should be observable within 1-2 weeks and attributable to the launch.)

**Common mistake:** Choosing a metric the launch cannot influence. If the success signal is "pipeline signups increase 20%," but signups depend on a sales process that takes 60 days, the signal does not measure the launch. It measures something that started before the launch and will continue after.

## How the brief connects to other GTM artifacts

The brief is upstream of everything:

- **Blog post.** Expands the claim and narrative arc into a full piece. The claim is the headline or first paragraph. The proof points structure the body. The entry point is this blog post.
- **Email announcement.** A compressed version of the brief — claim, one proof point, link to entry point. The primary audience determines the email list.
- **Social posts.** The claim, adapted to the platform's format. Channel plan rows map to individual posts.
- **Sales enablement.** The claim and proof points, reframed for buyer conversations. The "what we are not saying" section is especially important here — sales teams will otherwise make the claims the brief excludes.
- **Internal announcement.** The brief itself, shared as-is. The team should see the same strategic document the market-facing work derives from.

The brief does not replace any of these artifacts. It is the shared reference that makes them consistent. If the blog post says something the brief does not, one of them is wrong.

## What to do when there is too much to fit

If the brief exceeds one page, diagnose which section is overflowing:

- **Primary audience is too long.** The audience is not specific enough. Return to AUDIENCE-GRILLING.md.
- **The claim is too long.** It is trying to say two things. Pick the stronger one.
- **Too many proof points.** Four or more means the team is not confident in any single one. Pick the two strongest.
- **Channel plan is too long.** Cut to 2-3 channels. The rest can happen, but they are not in the brief.
- **"What we are not saying" is too long.** Good — it means the team identified many failure modes. Compress to the top 3 most tempting off-message claims.
- **Everything is overflowing.** The launch is too big. Split into two briefs.
