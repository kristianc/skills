# AUDIENCE-GRILLING.md

The audience grilling is the load-bearing step of the launch brief. Everything downstream — message, channel, format, timing — is determined by who the audience is. A vague audience produces vague messaging that lands on no one. A specific audience produces a message so pointed it feels like it was written for the reader personally.

This file is the framework for that grilling. Use it when step 2 of SKILL.md is not converging, or when the user is resisting specificity.

## Why audience comes before message

It is tempting to start with the message — "we want to say X" — and then figure out who to say it to. This is backwards, and it is the root cause of most launch messaging that falls flat.

The reason is structural: different audiences care about different things. A feature that saves an engineer three hours a week and saves a VP $200k/year in headcount is the same feature, but the message is completely different. The proof points are different. The narrative arc is different. The channel is different. The format is different.

If you write the message first, you are implicitly choosing an audience — you just have not named it. Unnamed audiences cannot be verified, cannot be narrowed, and cannot be wrong. They drift toward "everyone," which means "no one in particular."

The discipline is: name the audience first, verify it is specific enough, then let the message follow from what that audience cares about.

## The five questions

Ask these in order. Each one narrows the audience.

### 1. Who is the primary audience?

Not "our users." Not "developers." Not "small businesses." Which users? Which developers? Which small businesses?

The test: can you picture a specific person? Not a persona document — a real human being you could point to and say "this launch is for them." If you cannot picture someone specific, the audience is still too broad.

Push the user with:

- "Which of your current users would you most want to see this announcement?"
- "If you could only tell 100 people about this, who would they be?"
- "Is this for people who already use the product, or people who do not yet?"
- "Is this for the person who decides to buy, the person who uses it daily, or the person who implements it?"

These are different people with different information needs, different channels, and different proof points. A launch aimed at all three is aimed at none.

### 2. What do they care about right now?

Not what they care about in general — right now. What problem are they actively trying to solve? What frustration are they living with today?

This question is important because a launch only lands if it intersects with something the audience already wants. You are not creating desire; you are connecting to desire that already exists. The launch is interesting because it solves a problem the audience already has, not because the product team thinks it is impressive.

Push the user with:

- "What would this person complain about at lunch?"
- "If they could fix one thing about their current workflow, what would it be?"
- "What did they ask for in the last support ticket, feature request, or sales call?"
- "What are they searching for — literally, what search terms are they typing?"

If the user does not know what the audience cares about right now, the grilling should pause. Go find out. Read support tickets, sales call notes, community threads, competitor reviews. The launch can wait; launching without this knowledge cannot be fixed after the fact.

### 3. How does this launch connect to what they care about?

The launch is only interesting if it intersects with the answer to question 2. This question forces the connection to be explicit.

Push the user with:

- "If this person heard about the launch, what would they think first — 'finally,' 'interesting,' or 'so what'?"
- "Does this solve the problem they are actively dealing with, or a different problem?"
- "Is the connection obvious, or does it require explanation?"

If the connection requires more than one sentence of explanation, either the audience is wrong (this launch is actually for someone else) or the message needs reframing (the launch connects to what they care about, but not in the way the product team naturally describes it).

### 4. What would make them share this?

Not "it is cool." Sharing happens when the launch triggers a specific emotional reaction that the person wants others to know about. The common triggers:

- **"Finally."** They have been waiting for this. The share is vindication — "I told you this was needed."
- **"I have been asking for this."** Closely related to "finally," but more personal. They feel heard.
- **"This changes how I work."** The share is practical — they are telling colleagues about a tool change.
- **"This is impressive."** Technical admiration. Common in developer audiences. The share is "look at what is now possible."
- **"This is relevant to us."** The share is informational — forwarding to a team because it affects their work.

If the user cannot identify which reaction the launch should trigger, the message is not pointed enough. The reaction tells you the emotional register of the messaging.

### 5. Who is NOT the audience?

This question matters more than it appears. Every launch attracts attention from people it was not designed for. If you have not named who is not the audience, you will unconsciously dilute the message to avoid alienating them.

Common dilution patterns:

- Adding enterprise features to a launch aimed at individual developers, because "enterprise customers might see it too."
- Softening technical claims because "non-technical buyers might read it."
- Adding use cases that do not fit the primary audience because "we should mention those too."

Being explicit about who is not the audience gives permission to be specific. "This launch is for backend engineers; frontend engineers may find it interesting but we are not optimizing for them" is a useful statement. It prevents the channel plan from including a design-focused newsletter, prevents the proof points from including a UI screenshot, and prevents the message from hedging with "whether you work on the frontend or backend."

## Primary vs. secondary audiences

A launch has one primary audience and at most one or two secondary audiences. The distinction matters:

- **Primary audience:** The message is written for them. The claim addresses what they care about. The channels are where they are. The proof points are what would convince them. The brief is about them.
- **Secondary audience:** They may see the launch and find it relevant, but the message is not optimized for them. They benefit from the launch landing well with the primary audience — enthusiasm spreads outward.

The mistake is treating two audiences as co-primary. This produces messages with two claims, proof points that serve different people, and channels that split attention. The result is two half-launches instead of one full one.

If the user genuinely cannot choose a primary audience, the launch is probably two launches. Split them. Two briefs, two moments, two messages.

## How to know the audience is specific enough

The audience definition is specific enough when:

1. **The message writes itself.** If you know exactly who you are talking to and what they care about, the headline claim is almost obvious. If you are struggling with the message, the audience is usually too broad.
2. **You can name the channel without thinking.** "Where do backend engineers who work with large datasets hang out?" has a clear answer. "Where do our users hang out?" does not.
3. **You can predict the reaction.** You can say "when they see this, they will think X" with confidence. If you cannot predict the reaction, you do not know the audience well enough.
4. **The audience can be wrong.** "Developers" cannot be wrong — it is too broad to verify. "Python developers who currently use pandas for data pipelines over 1M rows and have hit performance limits" can be wrong. It might turn out those developers do not care about this launch. That is fine — being wrong is how you learn. Being unfalsifiable is how you stay ignorant.
5. **The "not the audience" list is non-trivial.** If nobody is excluded, the audience is "everyone." A specific audience necessarily excludes people who might seem relevant.

## Vague vs. specific: examples

**Vague:** "Developers who use our API."
**Specific:** "Developers who have made more than 1,000 API calls in the last month and have hit rate limits at least once."
**Why it matters:** The vague audience gets a message about "improved API." The specific audience gets a message about "rate limits are 10x higher" — which is the message that makes them stop scrolling.

**Vague:** "Small businesses."
**Specific:** "Solo founders running a SaaS product who currently handle billing manually because existing tools are too expensive or too complex for one person."
**Why it matters:** The vague audience gets a generic product announcement. The specific audience gets "you can stop manually reconciling Stripe invoices" — which is the sentence that makes them click.

**Vague:** "Our enterprise customers."
**Specific:** "Security teams at companies with SOC 2 requirements who currently spend 20+ hours per quarter on compliance reporting using spreadsheets."
**Why it matters:** The vague audience gets a feature list. The specific audience gets "compliance reports in one click, formatted for your SOC 2 auditor" — which is the sentence that gets forwarded to the CISO.

## When the grilling stalls

If the audience grilling is not converging, usually one of these is true:

- **The user does not know their audience.** This is not a failure of the grilling — it is a real finding. The launch is not ready. Recommend talking to 5 customers before writing the brief.
- **The launch is actually for internal alignment.** Some "launches" are really about making the team feel good about shipping. The audience is the company, not the market. This is fine, but the brief should be honest about it — and the channel plan looks very different.
- **The launch is too big.** It covers too many changes for one audience to care about all of them. Split into multiple launches, each with its own audience and brief.
- **The user is afraid of being specific.** Specificity feels like leaving money on the table. It is not. A launch that lands with 1,000 of the right people creates more momentum than one that washes over 100,000 of the wrong people. Enthusiasm is contagious; indifference is not.
