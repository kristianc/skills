# CHANNEL-SELECTION.md

Channels are where the audience encounters the launch. The most common mistake in launch planning is choosing channels by habit — "we always do a blog post and a tweet" — instead of by audience. This file covers how to select channels based on where the audience actually pays attention, how to match format to platform, and how to scope the plan to what the team can actually execute well.

## The principle: channels follow audience

The channel decision is downstream of the audience decision. It is not "which channels should we use for this launch?" — it is "where does this audience already pay attention, and how do we show up there?"

This is not a subtle distinction. It changes the plan completely:

- **Habit-driven:** "We will write a blog post, tweet about it, and send an email."
- **Audience-driven:** "Our primary audience — Python developers hitting pandas performance limits — hangs out in r/dataengineering, reads the Data Engineering Weekly newsletter, and follows certain accounts on Twitter/X. We will write a blog post as the entry point, pitch a mention in Data Engineering Weekly, and post a technical breakdown on r/dataengineering."

The habit-driven plan is generic. It would be identical for any launch, any audience, any product. The audience-driven plan is specific — it could only work for this audience, which is why it will.

## Identifying where the audience pays attention

Start from the audience definition (AUDIENCE-GRILLING.md) and ask:

### Where do they go to learn about tools?

Not where you wish they went. Where they actually go. For many technical audiences, this is not your blog. It is:

- **Communities.** Subreddits, Discord servers, Slack communities, forums. These have high trust because content is peer-generated, not vendor-generated.
- **Newsletters.** Curated newsletters in their domain. The curator's endorsement carries weight that a tweet does not.
- **Conference talks and recordings.** If the audience attends specific conferences, a talk or mention there has outsized impact.
- **Individual voices.** Specific people the audience follows and trusts. An organic mention from a trusted voice in the space is worth more than a hundred impressions on your own channels.

### Where do they go when they have a problem?

This is different from where they learn. When they hit a problem — the problem your launch solves — where do they go?

- **Stack Overflow / GitHub Issues.** If the audience searches for the problem, they end up here.
- **Search engines.** What do they search for? The answer tells you what content to create (and what keywords to target in the entry point).
- **Internal Slack / Teams.** They ask a colleague. This means the launch needs to reach the colleagues who answer, not just the ones who ask — the "helpful person in every team" is a key distribution node.
- **Documentation.** Some audiences go straight to docs when they hear about a new capability. If so, the docs themselves are a launch channel.

### Where do they share things?

This is different from both of the above. When the audience finds something interesting, where do they share it?

- **Twitter/X.** Short-form, public sharing. Works when the claim is tweetable — specific enough to fit in a post and interesting enough to repost.
- **Slack / Discord.** Semi-private sharing with a team or community. Works when the claim is relevant to a group ("hey team, this solves our pipeline issue").
- **Email forwards.** The oldest and most reliable sharing mechanism. Works when the claim is relevant to a specific person ("thought of you when I saw this").

Knowing where they share tells you what format to optimize for. If sharing happens on Twitter, the launch needs a tweetable artifact (a compelling one-liner, a demo GIF, a benchmark comparison). If sharing happens via email forward, the entry point needs to stand on its own — no context required from the surrounding feed.

## Matching format to platform

Every platform has a format that earns attention and a format that gets ignored. Using the wrong format on the right platform wastes the effort.

### Twitter/X

**What earns attention:** Specific, surprising claims. Demo GIFs or short videos. Benchmark comparisons with numbers. Threads that tell a story with a strong first tweet.

**What gets ignored:** Announcement-style posts with no specificity ("We are excited to announce..."). Links without context. Self-congratulatory threads.

**The format question:** Can the claim fit in a single tweet? If not, does the first tweet of a thread stand alone — would someone who only sees the first tweet understand why they should care?

### Reddit / Forums

**What earns attention:** Technical depth. Honest, non-promotional framing. Showing the work — benchmarks, architecture decisions, tradeoffs acknowledged. Posts that start with the problem, not the product.

**What gets ignored:** Marketing copy. Posts that read like press releases. Anything that feels like it was written by a marketing team rather than an engineer.

**The format question:** Would this post be useful even if the reader never clicks the link? The best Reddit/forum posts deliver value in the post itself — the link to the product is a natural extension, not the point.

### Email (newsletter or announcement)

**What earns attention:** A clear subject line that names the benefit. One claim, one proof point, one link. Brevity — the reader decides in the first two sentences whether to continue.

**What gets ignored:** Long announcements with multiple features. Emails that require scrolling to find the point. Subject lines that are clever instead of clear.

**The format question:** If the reader reads only the subject line and the first sentence, do they know what this is and why they should care?

### Community Slack / Discord

**What earns attention:** Conversational framing. Short messages with a link. Context about why it is relevant to that specific community. Posted by a person, not a brand account.

**What gets ignored:** Long announcements. Cross-posted marketing copy. Messages from accounts that only appear to promote things.

**The format question:** Would a community member post this? If not, it will feel like an ad in a non-ad space.

### Blog (owned)

**What earns attention:** Depth. Technical detail. Benchmarks, comparisons, architecture decisions, honest discussion of limitations. The entry point for the launch — where all other channels point back to.

**What gets ignored:** Nothing — the blog post is the entry point, not a channel for discovery. It does not need to earn attention on its own; it needs to reward the attention that other channels direct to it.

**The format question:** Does this blog post fully deliver on the claim? If someone arrives skeptical, do they leave convinced? If someone arrives interested, do they leave knowing how to act?

## Scoping to 2-3 channels

The right number of channels is almost always 2-3. Not one — a single channel has no reinforcement. Not eight — the team cannot execute eight channels well, and the effort dissipates.

### How to choose

Rank candidate channels on two axes:

1. **Audience density.** What fraction of the primary audience is reachable through this channel? A subreddit with 200k subscribers, 80% of whom match the primary audience, is higher density than Twitter, where the primary audience is a small fraction of the total feed.
2. **Format fit.** Does the platform's natural format match the message? A claim that requires a demo GIF to be convincing is a bad fit for a text-only newsletter. A claim that requires technical depth is a bad fit for Twitter.

Pick the top 2-3 by the combination of density and fit. Cut the rest. They can happen informally, but they are not in the brief and they are not allocated team time.

### The 2-3 channel test

If the team could only do 2 channels and cancel the rest, which 2 would they keep? Those are the channels that belong in the brief. Everything else is either a nice-to-have or a distraction.

## The entry point

The entry point is the canonical asset that everything else points back to. It is usually a blog post or a landing page. Less commonly, it is a changelog entry, a documentation page, or a video.

### Why the entry point matters

Every channel-specific post is a compressed version of the launch message — adapted to the platform, shortened for the format. The entry point is the uncompressed version. It is where the full claim lives, where all the proof points are presented, and where the audience can go deeper.

Without an entry point, each channel-specific post must stand entirely on its own. This means either every post is too long (trying to include everything) or every post is too shallow (missing proof that would make the claim credible). The entry point solves this by being the place that has everything — channel posts can be sharp and specific because the entry point carries the depth.

### Choosing the entry point

The entry point should be:

- **Controllable.** The team can update it, fix errors, and add context after launch. A tweet cannot be edited; a blog post can.
- **Linkable.** It has a stable URL that every channel can point to.
- **Complete.** It contains the full claim, all proof points, and enough context for a reader who arrives with no prior knowledge of the product.
- **Findable.** It should be indexed and searchable for the terms the audience would use to find the solution.

### Common entry point mistakes

- **No entry point at all.** Channel posts link to the homepage. The audience arrives and has to figure out what changed — the launch message evaporates.
- **The entry point is a tweet or social post.** These are ephemeral, uneditable, and cannot carry the depth the claim requires.
- **The entry point is behind a login.** The audience cannot access it without an account. The launch dies at the front door.

## Timing considerations

### Sequencing channels

Not all channels need to fire simultaneously. A common pattern:

1. **Entry point goes live.** Blog post or landing page published. Not promoted yet — this is the canonical asset being placed.
2. **Owned channels first.** Email to existing users, in-app announcement. These are the people most likely to care and most likely to amplify.
3. **External channels second.** Community posts, newsletter pitches, social media. These reach the broader primary audience, and they benefit from the early signal (existing users reacting, sharing, providing social proof).

The gap between steps is usually hours, not days. But the sequencing matters — an external post that goes live before the entry point is published sends traffic to a dead link.

### Day and time

Consider when the primary audience is paying attention:

- **Developer audiences:** Weekday mornings, when they check feeds before deep work.
- **Enterprise audiences:** Tuesday through Thursday, avoiding Monday inbox triage and Friday wind-down.
- **Consumer audiences:** Varies widely; match the platform's peak engagement hours.

Do not over-optimize timing. The difference between launching at 9am and 11am is negligible compared to the difference between a strong claim and a weak one. Get the message right first; optimize timing second.

### Avoiding collisions

Check for major events, competitor launches, holidays, and industry conferences that would drown out the announcement. A launch during a major conference works if the audience is at the conference. A launch during a conference the audience does not attend is wasted — no one is paying attention to their feed.

## The channel plan table

The deliverable for this step is a table in the brief:

```
| Channel              | Format            | Audience segment              | Key message angle                | Timing       |
|----------------------|-------------------|-------------------------------|----------------------------------|--------------|
| Blog (entry point)   | Long-form post    | Primary: [audience]           | Full claim + proof points        | D-day, 9am   |
| r/dataengineering    | Technical post    | Primary: [audience]           | Problem framing + benchmark      | D-day, 11am  |
| Twitter/X            | Thread + demo GIF | Primary + secondary           | Claim + visual proof             | D-day, 10am  |
```

Each row answers: where, in what format, for whom, saying what angle, and when.

Keep it to 2-4 rows. If the table has more, the team is overcommitting. Cut to the channels that have the highest audience density and format fit. The rest can happen, but they are not in the brief.
