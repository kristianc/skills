# COPY-PATTERNS.md

How to write good empty state copy, organized by type. This is not a template library — it's a set of principles and patterns with concrete examples. Every product's voice is different; these patterns work across voices.

## Anatomy of an empty state

Every empty state has up to four components. Not all are required for every instance.

### Headline

The first thing the user reads. It carries the orientation load — what this area is for and why it's empty. Short: 4-10 words. Written as a statement, not a question (questions imply the product doesn't know the answer either).

**Pattern:** [What] + [why empty], or [Status of the situation].
- "No tasks yet" — what + first-run.
- "All tasks complete" — status after clearing.
- "No results for 'acme'" — what + filtered cause.
- "Couldn't load your tasks" — status + error.

**Anti-pattern:** Feature name only ("Tasks"), generic absence ("Nothing here"), or system language ("0 items returned").

### Body text

One sentence of context, when the headline alone isn't enough. The body adds nuance — what this area does, what caused the empty state, what the alternative is. If the headline and action together are sufficient, skip the body.

**Pattern:** Explain value or cause, not mechanics.
- "Reports let you track metrics across your projects." — value.
- "This usually resolves in a few minutes." — cause + timeline.
- "Archived tasks are still in your archive." — where things went.

**Anti-pattern:** Repeating the headline in longer form. Explaining how the feature works in detail (save that for docs). Multi-sentence paragraphs — if you need more than one sentence, the empty state is doing too much.

### Action label

The button or link text. This is the most important word on the screen. It should be a verb phrase that tells the user exactly what clicking it does.

**Pattern:** [Verb] + [object], 2-4 words.
- "Create a report"
- "Clear filters"
- "Try again"
- "Request access"
- "Import contacts"

**Anti-pattern:**
- "Get started" — vague; started doing what?
- "Learn more" — as the only action, this is an escape, not a step. Acceptable as a secondary action alongside the primary.
- "OK" / "Got it" — dismissals, not actions.
- "Click here" — never.
- Single-word labels that don't specify the object: "Create" (create what?), "Retry" is the one exception — it's clear enough alone in error states.

### Illustration

A visual element — icon, illustration, or graphic. Optional and often unnecessary. When used well, it reinforces the message. When used poorly, it's decoration that adds load time and visual noise.

**When illustration helps:**
- First-run empty states on main feature screens — a simple graphic of the feature in use helps the user picture the populated state.
- Cleared states with a positive valence — "inbox zero" illustrations reinforce the accomplishment.
- Permissions states — an icon (lock, shield) quickly signals the nature of the block.

**When illustration hurts:**
- Error states — a cute illustration next to "Something went wrong" undermines the seriousness. The user wants a fix, not a cartoon.
- Every empty state in the product uses the same generic illustration — if the picture doesn't add information specific to this screen, it's filler.
- The illustration shows the feature's populated state in detail, making the empty state feel like a taunt rather than a beginning.
- The illustration takes longer to load than the data would have.

**Rule of thumb:** If you covered the illustration, would the empty state be worse? If not, cut the illustration.

---

## Patterns by type

### First-run copy

**Job:** Welcome and activate. The user has never been here; get them started.

**Headline pattern:** "[Your Xs] will appear here" or "No [Xs] yet."
- "Your reports will appear here." — forward-looking, implies the user will have them.
- "No tasks yet." — direct, implies this is temporary.
- Avoid: "Welcome to Reports!" — the user doesn't need a greeting, they need a direction.

**Body pattern:** One sentence of value proposition, if the feature isn't self-explanatory.
- "Track metrics across all your projects in one place."
- Skip the body if the feature name plus headline are enough: "No tasks yet" on a screen titled "Tasks" needs no body.

**Action pattern:** The creation action, labeled specifically.
- "Create a report" — not "Get started," not "New," not "Add."
- If there are two entry points (create from scratch, import), show both. Primary gets the button, secondary gets a text link.

**Voice notes:**
- Tone is forward-looking, not apologetic. "Not yet" is better than "you don't have any."
- Never imply the user did something wrong by not having items.
- Don't be too eager — "Let's create your first report!" has an energy that many products don't warrant.

### Cleared copy

**Job:** Confirm and redirect. The user took action; acknowledge it and show what's next.

**Headline pattern:** "[Status reflecting the action]" or "All [Xs] [past participle]."
- "All tasks complete." — reflects what the user did.
- "Inbox zero." — if the product's voice is casual enough.
- "No open items." — clinical but accurate.
- Avoid: "No tasks." — doesn't acknowledge that the user had tasks and completed them.

**Body pattern:** Where things went, or what to do next.
- "Completed tasks are in your archive."
- "New messages will appear here."
- Often no body is needed — the headline + the natural next action in the UI is enough.

**Action pattern:** The natural follow-up, or the path to what was removed.
- "View archive" — if the cleared items are recoverable.
- "Create a new task" — if the user might want to continue working.
- Sometimes no action is needed. "All caught up" with no button is a valid and satisfying state.

**Voice notes:**
- Tone matches the emotional valence. Completing tasks is positive; deleting a project is neutral. Don't celebrate a deletion.
- Never show the first-run message to a user who just cleared. "Create your first task!" after completing all tasks is jarring.

### Filtered copy

**Job:** Explain and recover. The user's query returned nothing; help them adjust.

**Headline pattern:** "No [Xs] matching [constraint]" or "No results for '[query]'."
- "No contacts tagged 'VIP'." — mirrors the filter.
- "No results for 'acme'." — mirrors the search query.
- Always include the user's input in the headline. If they can't see what they searched for, they can't judge whether to refine.

**Body pattern:** Context about total count, or a suggestion.
- "12 contacts match other tags." — shows data exists.
- "Check the spelling or try a broader search." — when no smart suggestion is available.
- "Did you mean 'Acme Corp'? (12 results)" — when the system can suggest.

**Action pattern:** Clear the constraint, always. Plus a suggestion if available.
- "Clear filters" / "Clear search" — primary action, always present.
- "[Show all 347 items]" — when the total is known.
- Suggested alternatives as links, if the system can generate them.

**Voice notes:**
- Tone is helpful, not apologetic. The search didn't fail — it succeeded with zero results. "Sorry, no results" is wrong.
- Never offer a creation action as the primary path. The user was looking for something, not making something. "No results for 'acme' — Create a new acme?" is almost always wrong.
- The one exception: if the search is clearly a creation intent ("user searches for a project name that doesn't exist"), some products offer "Create 'acme'?" — but this is a product decision, not a default.

### Error copy

**Job:** Explain and recover. Something broke; be honest and offer a fix.

**Headline pattern:** "Couldn't [verb] your [Xs]" or "Something went wrong loading [Xs]."
- "Couldn't load your messages." — clear, specific, takes responsibility.
- "Couldn't connect to the server." — when the cause is known.
- Avoid: "Something went wrong." — too vague alone, but acceptable as a fallback when the system genuinely doesn't know what failed.

**Body pattern:** Cause if known, timeline if available, reassurance if warranted.
- "This usually resolves in a few minutes." — when it's a known transient issue.
- "Your data is safe — we just can't display it right now." — when data loss might be a concern.
- Skip the body if the retry button is sufficient context.

**Action pattern:** Recovery, scaled to the error.
- "Try again" / "Retry" — for transient errors.
- "Refresh the page" — when local state might be stale.
- "Check our status page" — as a secondary action for persistent errors.
- "Sign in again" — for auth errors.

**Voice notes:**
- Tone is honest and calm. Not apologetic (don't say "Sorry!" or "Oops!"), not dismissive ("Please try again later"), not overly technical ("Request timed out after 30000ms").
- Use "we" for system failures: "We couldn't load..." — the product takes ownership.
- Use "your" for the user's content: "your messages," "your data" — it reassures them their stuff exists.
- Error states are where voice gets stress-tested. A product that's warm everywhere but cold in errors doesn't actually have a voice — it has a veneer.

### Permissions copy

**Job:** Explain the boundary and offer a path through it.

**Headline pattern:** "[Xs] are [access description]" or "You don't have access to [Xs]."
- "Reports are available on the Team plan." — what + why.
- "You don't have access to this project." — direct.
- Avoid: "Upgrade to unlock reports!" — this is marketing copy, not product copy. The user is inside the product; they need information, not a pitch.

**Body pattern:** The alternative or the path to access.
- "Ask your workspace admin for access." — when it's role-based.
- "Your current metrics are available on the dashboard." — when there's a free alternative.
- "Taylor (workspace admin) can grant access." — when the system knows who the admin is. Always name the person if you can.

**Action pattern:** The step that resolves the access issue.
- "Request access" — sends a request to the admin.
- "See plans" / "Compare plans" — for plan-gated features. Neutral labeling, not "Upgrade now!"
- "Contact [admin name]" — when the system knows who to contact.

**Voice notes:**
- Tone is matter-of-fact. Not apologetic ("Sorry, you can't access this"), not exciting ("Unlock this feature!"), not condescending ("You'll need to ask your admin").
- Never make the user feel locked out or punished. They're encountering a boundary, not committing an error.
- If the product has a free tier and a paid tier, permissions copy is where the relationship between them is felt. Aggressive upselling here poisons the free experience. Respect earns upgrades; pressure loses trust.

---

## Cross-cutting concerns

### Consistency across types

The same screen may show different empty state types at different times. The headline structure, action placement, and tone should be consistent in layout but distinct in content. If the first-run headline is in the center of the screen, the error headline should be too — but the words and actions change.

A user who encounters the same screen in multiple states should feel like the product knows the difference, not like it's playing Mad Libs with a single template.

### When the product has no established voice

If CONTEXT.md doesn't exist or doesn't define voice, derive the voice from the product's best existing copy — its marketing site, its onboarding flow, its settings labels. Look for: formality level, sentence length, use of "you" vs. "your" vs. imperative, attitude toward the user (peer, helper, tool).

If even that's ambiguous, default to: direct, no exclamation marks, no apologies, active voice, 2nd person, contractions okay. This is the "competent colleague" default — it works for most products and offends none.

### Copy length

Empty states should be scannable in under 3 seconds. If the user needs to read a paragraph to understand what to do, the empty state is too long. Targets:

- **Headline:** 4-10 words.
- **Body:** 0-15 words (often zero).
- **Action label:** 2-4 words.
- **Total text on screen:** Under 30 words.

If you're over 30 words, something is wrong — either the empty state is doing too much (split it), or the copy is explaining instead of directing (cut to the action).

### Localization

Keep copy simple for translation. Avoid idioms ("all caught up"), wordplay, and culturally specific references unless the product is English-only by design. Prefer concrete language that translates literally: "All tasks complete" travels better than "Inbox zero."

Action labels are especially sensitive — "Get started" is vague in English and worse in translation. "Create a report" translates cleanly in nearly any language.

### Accessibility

Empty states must be accessible. This means:

- The empty state is announced to screen readers. A visually empty area that doesn't announce itself is an invisible dead end for non-sighted users.
- The action is a real button or link, not a styled div. Keyboard users need to tab to it.
- If an illustration is used, it has appropriate alt text or is marked decorative. Don't make a screen reader describe a cartoon mailbox.
- Color is not the only signal. An error empty state that relies on a red background to communicate "error" fails for colorblind users. The headline must carry the meaning.
