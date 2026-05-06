# SCORING-RUBRIC.md

The three axes for evaluating an empty state. Each axis is scored 1-5. Anything below 3 on any axis is a candidate for rewrite. The axes are independent — a well-oriented empty state can still score 1 on voice, and a beautifully written one can still be a dead end.

## Axis 1: Orientation

Does the user understand what this area is for and why it's empty?

### Score 1 — No context

The user cannot tell what belongs here or why it's empty. The screen is blank, or shows only a generic icon. No headline, no explanation, or a headline so generic it could appear anywhere in the product.

**Examples at this level:**
- A completely blank white area below a navigation tab.
- An icon of an empty box with no text.
- "No data."

**Why this is a 1:** The user must use surrounding navigation, page titles, or memory to understand what they're looking at. The empty state itself communicates nothing.

### Score 2 — Feature name only

The empty state names the feature but doesn't explain the value or why it's currently empty. The user can identify where they are but gets no help understanding what to expect.

**Examples at this level:**
- "Reports" as a headline over a blank area.
- "No reports." — names the thing but provides no context.
- "Your dashboard is empty." — locates the user but says nothing about what a dashboard does or why this one is empty.

**Why this is a 2:** The user knows the feature name but still has to guess what the feature does, what would appear here, and whether "empty" is expected or broken.

### Score 3 — Clear what, unclear why

The user understands what type of content belongs here but not why it's currently absent. The empty state describes the feature adequately but doesn't distinguish between "you haven't created anything," "you deleted everything," "nothing matched your search," or "something broke."

**Examples at this level:**
- "Reports you create will appear here." — clear what, but shown identically whether the user has never created a report, just deleted their last one, or has a filter active.
- "No matching items." — clear that a filter is involved, but doesn't show what filter.

**Why this is a 3:** The user is oriented to the feature but not to their situation. They may take the wrong next step because the empty state doesn't reflect the cause.

### Score 4 — Clear what and why

The empty state communicates what belongs here, why it's empty, and what the user's current situation is. It distinguishes between empty-state types appropriately. The user is rarely confused.

**Examples at this level:**
- "No reports yet. Create one to start tracking your metrics." — clear it's first-run, clear what reports do.
- "No results for 'acme'. Clear search to see all 34 contacts." — clear it's filtered, mirrors the query, shows the total.

**Why this is a 4 (not 5):** The orientation is functional but doesn't use everything the system knows. It might not mention the number of items in other states, or might not differentiate between closely related empty types.

### Score 5 — Full situational context

The empty state uses all available system context to orient the user precisely. It knows whether this is their first visit, what they just did, what filters are active, and what the system's state is. The user never has to guess.

**Examples at this level:**
- First visit: "Your reports will appear here. Reports let you track metrics across your projects — create your first one to get started." Subsequent clear: "All reports archived. View archived reports or create a new one."
- "No contacts match 'acm'. Did you mean 'acme' (12 contacts)?" — mirrors the query, suggests correction, shows what the correction would yield.

**Why this is a 5:** The empty state is aware of the user's history, the current context, and the system state. Nothing is generic.

---

## Axis 2: Next action

Does the empty state offer the user a clear next step?

### Score 1 — Dead end

No action is offered. The user must figure out where to go and how to get there on their own. There is no button, no link, no suggestion.

**Examples at this level:**
- "No items." and nothing else.
- A blank area with no interactive elements.
- "Something went wrong." with no retry, no refresh, no help link.

**Why this is a 1:** The user is stranded. Their only options are to navigate away, refresh the page, or give up. The product has abandoned them mid-flow.

### Score 2 — Vague suggestion

The empty state suggests what to do in prose but doesn't provide a direct action. The user has to translate the suggestion into navigation.

**Examples at this level:**
- "Try creating a report from the Reports menu." — tells the user to go somewhere else but doesn't take them there.
- "Check your filters." — names the problem without offering the fix.
- "Contact your administrator for access." — but doesn't identify the administrator or offer a contact action.

**Why this is a 2:** The user knows what to do in theory but must figure out the mechanics. The empty state describes the answer without providing it.

### Score 3 — Action present but generic

There is a clickable action, but it doesn't precisely match the user's situation. A "Create new" button on a filtered empty state. A "Learn more" link when what the user needs is a retry button.

**Examples at this level:**
- Filtered empty showing "Create a new report" when the user was searching, not creating.
- Error empty showing "Go to dashboard" when the user wanted to see this specific page.
- "Learn more" as the only action, linking to documentation when the user needs to take an in-product step.

**Why this is a 3:** An action exists, and the user can do something, but the action doesn't address the actual situation. It's a generic escape hatch, not a considered next step.

### Score 4 — Right action, offered clearly

The action matches the user's situation and is easy to take. First-run shows "Create," filtered shows "Clear filters," error shows "Retry." The button or link is prominent and well-labeled.

**Examples at this level:**
- "No reports yet. [Create a report]" — correct action for first-run.
- "No results for 'acme'. [Clear search]" — correct action for filtered.
- "Couldn't load messages. [Try again]" — correct action for error.

**Why this is a 4 (not 5):** The primary action is right, but it doesn't offer alternatives or use system context to anticipate. The filtered empty could show "Did you mean 'acm'?" The error could offer "Retry" and "Check status page." The first-run could offer "Import from CSV" alongside "Create."

### Score 5 — Right action with alternatives

The primary action matches the situation, and secondary actions anticipate related needs. The empty state functions as a micro-navigation, not just a single button.

**Examples at this level:**
- First-run: "[Create a report] or [Import from CSV]" — two paths to populate.
- Filtered: "[Clear search] — or try: 'Acme Corp' (12 results), 'Acme Inc' (3 results)" — clear plus suggestions.
- Error: "[Try again] — or [check our status page] if this keeps happening." — immediate fix plus escalation.
- Permissions: "[Request access] — or ask Taylor (workspace admin) directly." — action plus the specific person.

**Why this is a 5:** The user has a primary path and a secondary path, both relevant to their situation. The empty state doesn't just offer escape — it offers the most useful escape for this specific moment.

---

## Axis 3: Voice

Does the empty state sound like the product?

### Score 1 — System speak

The copy reads like a developer wrote it for a log file. Technical jargon, error codes, passive voice, no personality. Or: the copy is so generic it could appear in any product without change.

**Examples at this level:**
- "Error: ECONNREFUSED"
- "No records found."
- "0 results returned for query."
- "Items: (empty)"
- "null"

**Why this is a 1:** The user is reading system internals. This copy wasn't written for them — it was written for a console, and it leaked into the UI.

### Score 2 — Functional but characterless

The copy is in human language and makes sense, but it has no personality, no awareness of the product's voice, and sounds interchangeable with any other product. It was written to be correct, not to be good.

**Examples at this level:**
- "There are no items to display."
- "No results found. Please try again."
- "You don't have any reports."
- "This folder is empty."

**Why this is a 2:** The user understands the message. It doesn't confuse or alarm. But it also doesn't sound like anyone wrote it with care. It's the copy equivalent of a default browser font.

### Score 3 — Approaching voice

The copy is more natural and shows some attention to tone, but it inconsistently matches the product's voice. It might use the right vocabulary but wrong formality, or the right formality but wrong attitude.

**Examples at this level:**
- A direct, no-nonsense product: "Hmm, looks like there's nothing here yet!" — the tone is friendly but the product's voice is terse.
- A warm product: "No reports." — the product's voice is warm but the copy is cold.
- Using "items" when the product calls them "tasks" — right tone, wrong vocabulary.

**Why this is a 3:** You can see the attempt. The copy was written for humans, by someone who cares, but it doesn't match the specific voice of this product. It's good copy in the wrong product.

### Score 4 — In voice

The copy matches the product's established voice — vocabulary, formality, attitude. It reads as if the same person who wrote the product's best copy also wrote this empty state. If CONTEXT.md defines the voice, this copy follows it.

**Examples at this level:**
- A direct product's first-run: "No tasks yet. Create one." — terse, imperative, no filler.
- A warm product's first-run: "Your tasks will show up here once you create them. Ready to add your first one?" — conversational, inviting, still clear.
- Uses product vocabulary consistently ("threads" not "conversations," "workspace" not "account").

**Why this is a 4 (not 5):** The voice is correct, but the copy doesn't quite earn its place the way the product's best lines do. It's in voice but not memorable. It does the job without making the user feel like the product knows their situation.

### Score 5 — In voice and situationally aware

The copy matches the product's voice and also reflects the specific moment — what the user just did, what they're trying to do, what the system knows. The tone shifts appropriately between first-run optimism, cleared-state satisfaction, filtered helpfulness, error candor, and permissions explanation.

**Examples at this level:**
- A direct product's error state: "Couldn't load your tasks. Usually back in a few minutes. [Retry]" — terse, honest, no apology, specific.
- A warm product's cleared state: "All done for today. Your completed tasks are in the archive if you want to revisit." — warm, acknowledges the accomplishment, offers the natural follow-up.
- A professional product's permissions state: "Analytics are available on the Team plan. Your current metrics are on the dashboard." — no upsell pressure, offers the alternative, respects the user's current situation.

**Why this is a 5:** The copy is doing double duty: matching the voice and adapting to the moment. The user feels both that the product has a personality and that it knows their situation.

---

## Edge cases

### When axes conflict

Sometimes improving one axis hurts another. Common conflicts:

- **Orientation vs. voice:** Adding full context makes the copy long and dry. Resolution: use the headline for orientation and the body for voice. "No results for 'acme'" (orientation) + "Try a broader search or check the spelling" (voice and next action).
- **Next action vs. voice:** The most useful action label is "Clear filters," but the product's voice would say "Start over." Resolution: voice wins when both are clear. If voice makes the action ambiguous, clarity wins.
- **Voice vs. consistency:** The product's voice calls for warmth, but this is an error state and warmth reads as dismissive. Resolution: voice has registers. A warm product can be serious in error states without breaking voice. See COPY-PATTERNS.md for how tone shifts by type.

### Scoring shared components

If the same `<EmptyState />` component is used across 10 screens, score the worst instance, not the average. Generic components score low on all three axes because they can't be situationally aware. The recommendation is almost always: replace the generic component with type-specific treatments, or at minimum pass context props that allow the component to adapt.

### When 3 is fine

Not every empty state needs to be a 5. Rarely-visited screens, advanced settings panels, and administrative tools can live at 3-across without hurting the product's feel. The goal is to eliminate 1s and 2s everywhere, and push to 4-5 on the empty states users hit most: first-run, main feature areas, and high-traffic filtered views.

### Disagreements

If two people score the same empty state differently, the disagreement usually means one of them is imagining a different user scenario. Resolve by pinning the user state: "Score this as if the user just signed up." "Score this as if the user just deleted their last item." Context collapses most scoring disagreements.
