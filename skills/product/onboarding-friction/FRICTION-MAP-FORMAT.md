# FRICTION-MAP-FORMAT.md

The friction map is the primary deliverable of this skill. It's an ordered walk-through of the activation path, annotating every point where the product assumes knowledge the user doesn't have. This document specifies how to structure, order, and present it.

## What a friction map is

A friction map is not a bug list, not a priority matrix, and not a redesign spec. It's a sequential record of what a first-time user encounters along the shortest path from signup to activation, with each assumption surfaced and graded.

The audience is someone who will read it top to bottom, experiencing the path vicariously. If the map is ordered well, the reader finishes it thinking "I understand exactly where new users get stuck and why."

## Entry format

Each friction point is one entry. Every entry has five fields, always in this order:

### Step

Where in the activation path this occurs. Be specific enough that someone unfamiliar with the product can locate the moment.

- Good: "First screen after signup — the empty project list"
- Good: "Creating first project — the 'Select a Team' dropdown"
- Bad: "Onboarding" (too vague)
- Bad: "ProjectList component, line 47" (implementation detail, not user moment)

Use natural language that describes the user's experience, not the product's architecture.

### Assumption

What the product assumes the user already knows. Name the assumption type from ASSUMPTION-TYPES.md (vocabulary, workflow, mental model, prior-action, technical) and state the specific assumption.

- Good: "Vocabulary — assumes the user knows what a 'Pipeline' is in this product's context"
- Good: "Prior-action — assumes the user has already connected a data source, which happens in Settings > Integrations"
- Bad: "Confusing UI" (not an assumption)
- Bad: "User might not understand" (doesn't name what they don't understand)

### Evidence

What specifically in the UI reveals this assumption. Quote the copy, describe the control, point to the layout decision. The reader should be able to verify this by looking at the screen.

- Good: "The page heading reads 'Your Pipelines' with an empty table below. No explanation of what a pipeline is or why you'd want one."
- Good: "The 'Deploy' button is greyed out with no tooltip. The user needs to have configured an environment first, but nothing on this page mentions environments."
- Bad: "The UI doesn't explain it" (explain what?)

### Impact

What happens if the user doesn't have the assumed knowledge. Be specific about the consequence — not "the user is confused" but what the confusion leads to.

- Good: "The user skips this step and creates a project without a team, which means teammates can't see it. They discover this later and have to recreate the project."
- Good: "The user stares at an empty screen with no idea what to do next. There's no call to action and no explanation. Likely abandonment point."
- Bad: "Bad experience" (not specific)
- Bad: "User might get confused" (what does the confusion cause?)

### Severity

One of three levels. The level is about what happens to the user's progress, not how annoyed they feel.

**Blocking** — the user cannot proceed on the activation path without resolving this. They're stuck, and the product doesn't help them get unstuck. Examples: a required field with no explanation, an empty dropdown with no way to populate it, a concept they must understand to make a required choice.

**Confusing** — the user can proceed, but they proceed with uncertainty, make a wrong choice they'll have to undo later, or waste significant time figuring out what the product means. The activation path isn't broken, but it's damaged. Examples: a vocabulary term that's guessable but misleading, a hierarchy that's visible but unexplained, a setting that has a reasonable default but the user doesn't know whether to change it.

**Cosmetic** — the user notices the assumption but isn't meaningfully affected. They can continue without understanding the term, the convention, or the structure. Examples: jargon in a subtitle that doesn't affect the task, a breadcrumb that shows an unfamiliar hierarchy but the user only has one option at each level, a technical term in an error message that also includes the plain-language fix.

### Example entry

```
**Step:** First screen after signup — the empty dashboard

**Assumption:** Prior-action — assumes the user has installed the tracking snippet on their website

**Evidence:** The dashboard shows "Waiting for data..." with a loading spinner that never resolves. Below it, a small grey link reads "Setup guide" that leads to a developer-focused docs page about JavaScript snippet installation.

**Impact:** Non-technical users (the primary persona for this product) hit a dead end. They can't complete setup alone, so they need to ask a developer to install the snippet. Many won't — they'll leave and may not come back. The "Waiting for data..." framing also implies something is wrong, when in reality setup hasn't started.

**Severity:** Blocking
```

## Ordering entries

Entries are ordered by their position on the activation path, first to last. This is the walk-through order, not the priority order.

Start with the signup form (if it's part of the scope) and end at the activation moment. If the user never reaches activation because a blocking friction point stops them, the map ends at that point — and that's a key finding.

Within a single step (e.g., one screen that has three friction points), order from most prominent to least — what hits the user first visually, then what they encounter as they interact.

## Visualizing the path

Before the entry list, include a one-line-per-step summary of the activation path. This gives the reader the full journey before the detailed annotations.

```
Signup → Empty dashboard → Connect data source → First data arrives → First report created (activation)
```

Mark steps that contain blocking friction with an indicator:

```
Signup → Empty dashboard [2 blocking] → Connect data source [1 blocking] → First data arrives → First report created (activation)
```

This summary is the reader's map of the map. It should fit on a screen.

## Distinguishing blocking from cosmetic

The severity levels exist to prevent every friction point from feeling equally urgent. Apply them honestly.

### Blocking requires two conditions

1. The user cannot proceed without resolving the assumption, AND
2. The product doesn't give them enough information to resolve it

A term the user doesn't know isn't blocking if context makes the meaning clear enough to continue. A required choice isn't blocking if the options are self-explanatory. Blocking means stuck — the user has to leave the product (search the docs, ask someone, give up) to continue.

### Confusing is the most common and the most debatable

Most friction points are confusing, not blocking. The user gets through, but with uncertainty that compounds. The cost is: trust eroded, wrong choices made, time wasted, mental model built on sand.

When grading confusing, be specific about what the confusion leads to. "User might be unsure" is too weak. "User picks the wrong environment, builds on it for a week, then has to migrate" is concrete.

### Cosmetic is not "unimportant"

Cosmetic means the user's immediate progress isn't affected. It doesn't mean the friction doesn't matter. Five cosmetic friction points in a row make the product feel alien and uncaring — the same cumulative effect as five default touchpoints in polish work. Mark them cosmetic, but don't dismiss them.

## Compound entries

When a single step contains multiple assumption types (see ASSUMPTION-TYPES.md on compound assumptions), record it as one entry with the primary assumption in the Assumption field and note the compound in Evidence. Don't split a single user moment into multiple entries just because multiple assumption types are present — the user experiences it as one moment of confusion.

## What not to include

- **Feature requests.** "The product should also let users do X" is not a friction point. Stick to what's on the activation path.
- **Bugs.** Something that's broken is a bug, not an assumption. If the page doesn't load, that's not friction — that's a defect.
- **Preferences.** "I'd prefer a different layout" isn't an assumption the product makes. The core question is about knowledge, not aesthetics.
- **Expert friction.** If the target user profile established in Step 1 makes this knowledge reasonable to expect, it's not a friction point. Don't flag "API key" as friction for a developer audience.

## Updating the map

The friction map is a snapshot of a specific activation path for a specific user profile at a specific time. When recommendations are implemented, the map becomes outdated. If the product revisits onboarding later, walk the path again from scratch rather than updating the old map — assumptions shift as the product changes.
