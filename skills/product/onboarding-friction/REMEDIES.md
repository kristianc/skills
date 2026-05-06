# REMEDIES.md

Every friction point on the activation path gets one of four remedies. This document covers when to use each, when not to, the trade-offs, and the common mistakes. The hierarchy is: remove > teach inline > reorder > accept. Always try the higher remedy first and explain why it doesn't apply before reaching for a lower one.

## The hierarchy

The four remedies are ordered by how much friction they eliminate:

1. **Remove** — the friction point ceases to exist. The user never encounters it.
2. **Teach inline** — the friction point still exists, but the product gives the user what they need to get through it, right where they are.
3. **Reorder** — the friction point still exists, but it moves to after the user has enough context to handle it.
4. **Accept** — the friction point stays as-is. It's reasonable for the target user, or the cost of removing it exceeds the cost of keeping it.

Higher is better, but higher isn't always possible. The goal is to pick the highest remedy that actually works for the specific friction point, not to force every friction point into "remove."

---

## Remove

Eliminate the step, the choice, or the assumption entirely. The user never encounters the friction because the product resolves it without their involvement.

### When to use it

- The product can infer what the user would have chosen. (Default to the only workspace the user has, instead of asking them to select one.)
- The step can be deferred until the user actually needs it. (Don't ask for a billing address until the trial ends.)
- The information being requested is available from context. (Auto-detect timezone from the browser instead of asking.)
- The step exists for the product's convenience, not the user's. (Internal categorization that doesn't affect the user's experience.)

### When NOT to use it

- The choice is genuinely load-bearing and the product can't know the right answer. (Which team should own this project? The product can't guess.)
- Removing the step hides important information the user needs later. (Silently defaulting a privacy setting that the user would want to choose.)
- The inferred default is wrong often enough to cause more friction than asking. (Auto-detecting country by IP works 90% of the time, but the 10% have to find and fix it.)

### Before/after examples

**Before:** The first screen after signup asks "What's your role?" with six options. The answer determines nothing visible to the user — it's used for internal analytics.
**After:** The question is removed entirely. If the data is needed, collect it later in a non-blocking way (a later prompt, an optional profile section, a post-activation survey).

**Before:** "Select a timezone" dropdown with 400 entries on the signup form.
**After:** Timezone is auto-detected from the browser. A small "Change" link appears in settings for the rare user who needs to override it.

### Trade-offs

- Removing a step feels decisive but can mask important choices. If you're removing a choice the user should actually make, you're not reducing friction — you're hiding it.
- Inferred defaults need to be discoverable and changeable. The user who gets the wrong default and can't find where to change it has more friction than the user who was asked up front.

### Common mistakes

- Removing a step that serves a real purpose because it's inconvenient. The signup form asks for a company name because the product uses it everywhere — removing it means "Untitled Company" appears in 30 places.
- Confusing "remove the question" with "remove the concept." If the product still has Workspaces, removing the workspace creation step from onboarding just means the user encounters Workspaces later without having set one up.

---

## Teach inline

The friction point stays, but the product gives the user what they need to understand it, right where they encounter it. A line of copy, a contextual explanation, an empty state that teaches, a label that clarifies.

### When to use it

- The concept is real and the user needs to understand it eventually. (The product genuinely has Workspaces, and the user will interact with them often.)
- The assumption is vocabulary or mental model, and a sentence or two of context is enough. ("Pipelines are automated workflows that process your data. You'll create your first one now.")
- The step is necessary but the user doesn't know why. ("We need your billing address to calculate tax, even during the free trial" — a sentence turns a suspicious field into a transparent one.)
- The interaction uses a convention the target user might not know. (A drag handle icon next to reorderable items, or "Drag to reorder" in the first-use state.)

### When NOT to use it

- If the explanation is longer than a sentence or two, the concept is too complex to teach inline. Consider reordering so the user has more context first.
- If you're explaining something the product could have inferred or defaulted. Don't teach the user to make a choice the product could make for them — that's a "remove" in disguise.
- If the teaching requires the user to learn before doing. Inline teaching works when it's "learn by doing" — read the sentence, complete the step. If the user needs to study before proceeding, the step is in the wrong place (see "reorder").

### Before/after examples

**Before:** An empty project list that says "No projects."
**After:** "No projects yet. Projects are where you organize your work — create your first one to get started." with a prominent "Create project" button.

**Before:** A "Select Environment" dropdown showing "Development," "Staging," "Production" with no context.
**After:** The same dropdown, with a line above it: "Environments let you keep test data separate from real data. Start with Development — you can add more later." Development is pre-selected.

**Before:** A form field labeled "Slug" with no explanation.
**After:** The field is labeled "URL-friendly name" with helper text: "This becomes part of your project's web address. Letters, numbers, and hyphens only."

### Trade-offs

- Inline teaching adds words to the interface. For experienced users, those words are noise. Use progressive disclosure — show the teaching on first encounter, collapse it after.
- Inline teaching can become a crutch. If every step needs a paragraph of explanation, the product's information architecture may be the problem, not the copy. Five "teach inline" remedies in a row is a signal that the activation path needs restructuring.

### Common mistakes

- Writing the explanation in product-speak instead of user-speak. "Workspaces provide multi-tenant isolation for your organization's resources" teaches nothing. "A workspace is your team's private area — only people you invite can see what's inside" teaches.
- Adding a tooltip and calling it done. Tooltips are invisible until hovered, require a specific interaction to trigger, and disappear when the user moves their mouse. They're supplementary, not primary teaching. If the information is essential, it should be visible by default.
- Explaining the what without the why. "Pipelines process your data" — OK, but why do I want that? "Pipelines automatically clean and organize data as it comes in, so you don't have to do it manually" — now I understand the value.
- Placeholder text as teaching. Placeholder text disappears when the user starts typing, which is exactly when they might need to refer to it. Use labels and helper text that persist.

---

## Reorder

The friction point stays and the concept isn't changed, but the step moves to a point in the path where the user has enough context to handle it. Configuration after value, not before. Complex choices after simple ones.

### When to use it

- The step asks the user to make a decision they don't have context for yet. ("Configure your notification preferences" before the user has received a notification.)
- The step requires understanding a concept the product hasn't introduced yet. ("Select an environment" before the user has seen what environments do.)
- The step asks for configuration that only matters after the user has gotten value. (Billing info before the trial starts. Team settings before any collaboration.)
- The step would make sense after the activation moment, but the product front-loads it.

### When NOT to use it

- The step is a genuine prerequisite — you can't proceed without it, and it can't be defaulted. (You really do need to pick a name for your project before you can use it.)
- Reordering would create a new, worse assumption. (Moving "invite teammates" to after activation means collaborative features show empty states for longer.)
- The step is quick and low-friction in its current position. Don't over-optimize — if selecting a timezone takes 3 seconds and the user understands it, moving it to later doesn't improve the path.

### Before/after examples

**Before:** The signup flow asks for company size, industry, role, use case, and preferred integrations before showing the product.
**After:** Signup asks for name, email, and password only. The product asks about integrations when the user first tries to connect one. It asks about team size when the user first invites someone. Company industry is collected in a post-activation survey.

**Before:** The first screen after signup is a settings page: "Configure your workspace before getting started."
**After:** The workspace is created with sensible defaults. The user lands on the main product surface, and settings are accessible but not required up front.

**Before:** The product asks "Would you like to enable two-factor authentication?" during initial setup.
**After:** 2FA is prompted after the user has been active for a week, or when they first access a security-sensitive feature.

### Trade-offs

- Reordering can create a "deferred surprise" — the user encounters the complexity later, and they've already built something on top of the defaults. If the later encounter requires undoing work, the trade-off may not be worth it.
- Sensible defaults that the product ships with need to actually be sensible. A bad default that the user doesn't see until later is worse than a good question asked up front.
- Reordering changes the activation path, which means the friction map needs to be re-walked. A reorder isn't just moving a step — it changes the context of every subsequent step.

### Common mistakes

- Reordering everything behind the activation moment and ending up with a "day two cliff" — the user gets to value quickly but then hits a wall of deferred configuration.
- Deferring a step without defaulting it. If you move "Select timezone" to later, you need to auto-detect it now. If you move "Choose a plan" to after the trial, you need a default plan during the trial. Reorder without a default creates a hidden prerequisite.
- Using "reorder" as a way to avoid a hard product decision. If the step is confusing no matter where it appears, moving it later just delays the confusion. Fix the step.

---

## Accept

The friction point stays as-is. The assumption is reasonable for the target user, or the remedy would cost more than the friction does.

### When to use it

- The assumption genuinely matches the target user's knowledge. A developer tool can assume the user knows what an API key is. A design tool can assume the user knows what layers are.
- The cost of removing or teaching is disproportionate. Restructuring the data model to eliminate a hierarchy the user must learn might not be worth it when a sentence of inline teaching suffices — and if even teaching isn't worth it, accept.
- The friction is cosmetic and isolated. A single unfamiliar term in a subtitle that doesn't affect the user's path. An advanced setting page that new users never need to visit.
- Removing the assumption would patronize the target user. A product for data engineers that explains what SQL is would feel insulting.

### When NOT to use it

- The friction is blocking and the fix is straightforward. Never accept blocking friction that's solvable.
- The assumption isn't reasonable for the actual target user — it's reasonable for the user the team imagines. ("Our users know what a webhook is" — do they? Or do your power users know?)
- You're accepting because the fix is hard, not because the friction is small. Difficulty of the fix doesn't change the severity of the friction. Note it as a known issue with a hard fix — don't recategorize it as acceptable.

### How to document acceptance

An accepted friction point needs a reason, or the next review will re-flag it. Record:

- What the assumption is
- Why it's reasonable for the target user (be specific — "developers know this" is only valid if the target user is demonstrably a developer)
- Under what conditions this decision should be revisited (e.g., "If we expand to non-technical users, this becomes blocking")

### Before/after examples

**Before (audit flag):** "The CLI setup instructions say 'Add the binary to your PATH.' This assumes the user knows what PATH is."
**After (accepted):** "Accepted — the target user is a backend developer. Knowing what PATH means is table stakes. Revisit if we add a GUI installer for non-developer users."

**Before (audit flag):** "The API docs reference 'idempotency keys' without defining them."
**After (accepted):** "Accepted — our API is consumed by developers integrating payment systems, who universally encounter idempotency keys. The term is standard in this domain."

### Trade-offs

- Accepting is easy. Too easy. The risk is that "accept" becomes the default remedy for anything that's hard to fix. Counter this by requiring the documented reason — if you can't write a convincing reason, the acceptance isn't honest.
- Accepted friction points accumulate. Five individually reasonable assumptions, encountered in sequence, can make the product feel hostile even when each one is defensible. Review the full set of accepted items and check: is the cumulative load still reasonable?

### Common mistakes

- Accepting because "our users are smart." Smart users still encounter unfamiliar products. Intelligence doesn't substitute for domain knowledge.
- Accepting because the feature shipped already and changing it is politically hard. That's a resourcing constraint, not an acceptance rationale.
- Failing to specify a revisit condition. Accepted friction without a revisit condition is friction that will never be addressed, even when the conditions change.

---

## Choosing between remedies

Walk the hierarchy top to bottom for each friction point:

1. **Can the product resolve this without the user?** (Remove) — Infer, default, defer, auto-detect. If the product has the information or can get it without asking, the user should never see this step.
2. **Can a sentence or two of context make this clear?** (Teach inline) — If the concept is real and the user needs it, but the product just isn't explaining it, add the explanation where the user is.
3. **Would this make more sense later?** (Reorder) — If the user needs context they don't have yet, but will have after another step, move this step to after that context exists.
4. **Is this assumption actually reasonable?** (Accept) — If the target user genuinely has this knowledge, document why and move on.

If you find yourself reaching for "accept" on a blocking friction point, stop. Go back to the top of the hierarchy and try harder. Blocking friction that's accepted is a product that knowingly loses users at that step.

If you find yourself writing "teach inline" for five consecutive friction points, stop. The activation path may need restructuring (a reorder at the path level, not the step level) or the product's conceptual model may need simplification (a remove at the model level, not the step level).
