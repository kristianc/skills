# TAXONOMY.md

The five types of empty state. Each has a different user situation, different communication job, and different failure mode. Treating them interchangeably — one generic `<EmptyState />` component across the product — is the single most common reason empty states fail.

## 1. First-run empty

### The user's situation

They have never created, received, or configured anything in this area of the product. They may have just signed up, or they may be visiting a feature for the first time. They're oriented enough to have navigated here, but they don't yet know what the product will look like when it's full.

### What it needs to communicate

1. **What this area is for.** Not the feature name — the value. "This is where your reports live" is feature name. "Reports you create show up here" is closer — it tells the user what fills this space.
2. **That emptiness is expected.** The user should never wonder whether something is broken, still loading, or restricted. First-run empty must read unambiguously as "nothing yet," not "something went wrong."
3. **What to do first.** The single most important action — the one that turns this from empty to populated. A button, not a paragraph.

### What good looks like

- Headline explains the value of the area in the user's terms.
- A single, prominent call-to-action that does the thing: "Create a report," "Import your data," "Invite your team."
- If the feature is complex, a sentence of context — but only a sentence. Long onboarding tours belong elsewhere.
- The tone is confident and forward-looking. This is a beginning, not an absence.

**Example (good):** A project management tool's task list.
> **Your tasks live here.**
> Create your first task to get started.
> [New task]

### What bad looks like

- Just the feature name as headline with no action: "Tasks." Or worse: an icon and nothing else.
- "No tasks found." — found implies search, which implies something should have been here. Wrong mental model for first-run.
- An illustration of the feature in use, with no way to get to that state from here. Aspirational screenshots without a path are taunt, not onboarding.
- A wall of text explaining the feature. If the user needs a tutorial, surface it alongside the action, not instead of it.
- "Welcome! 🎉" — false cheer without orientation. The user still doesn't know what to do.

### Counter-example

**Not first-run:** The user had three tasks, archived them all, and now sees zero. That's cleared empty, not first-run. The distinction matters because the user already knows what this area is for — they don't need the welcome, they need confirmation that the archive worked.

### How to tell it apart

First-run empty is the only type where the user has *never* had content in this area. If the system can distinguish between "never had items" and "had items, now has zero," use different treatments. If the system can't distinguish, default to the first-run treatment — it's the safer failure mode (orientation is always useful; "welcome back to zero" is weird for a new user but not harmful).

---

## 2. Cleared empty

### The user's situation

They took deliberate action — deleted, archived, completed, dismissed — and now the list, inbox, or container is empty. They know what this area is for. They know it was full a moment ago. What they need is confirmation that the action worked and a sense of what comes next.

### What it needs to communicate

1. **The action landed.** The user should feel that the product acknowledges what they just did. "All caught up" after clearing notifications. "No open tasks" after completing them. The absence is the feedback.
2. **What to do next.** Sometimes the answer is "nothing — you're done." That's a valid message. Other times, there's a natural follow-up: "View archived items," "Create a new one."
3. **That this isn't permanent if they didn't mean it.** If the action is recoverable (archive, complete), a link to undo or view what was removed is high-value and low-cost.

### What good looks like

- Headline acknowledges the cleared state using language that reflects the action. "All tasks complete" after completing. "Inbox zero" after archiving. Not just "No items."
- If recoverable, a link to the archive or undo. Not prominent — the happy path is that they meant it — but accessible.
- The tone is satisfied, even slightly celebratory in products whose voice allows it. The user accomplished something.

**Example (good):** An email client after archiving everything.
> **All caught up.**
> Archived messages are in your archive.

### What bad looks like

- The same first-run message ("Create your first task!") shown after the user completed all their tasks. This ignores the user's history and reads as amnesiac.
- "No items found" — again, "found" is search language, not clearing language.
- No acknowledgment of the action at all — just a blank screen. The user wonders if the delete actually worked or if the page failed to load.
- Making the user feel bad about having zero items. "You have no projects. Get started!" to a user who just completed a project and is done for the day.

### Counter-example

**Not cleared:** The user's inbox is empty because the system failed to load messages. That's error empty, even though it looks identical. The distinction requires checking whether the API returned success-with-zero-results or failure.

### How to tell it apart

Cleared empty follows a user-initiated action that reduced the count to zero. The system knows the user had items and now doesn't. If you can track the action (delete, archive, complete, dismiss), you can distinguish this from first-run. If you can't, first-run treatment is the safer fallback — but invest in the distinction. Cleared empty that feels like first-run is one of the most common "this product doesn't know me" moments.

---

## 3. Filtered empty

### The user's situation

They applied a search query, filter, date range, tag, or other constraint — and nothing matched. They're looking for something specific, or they're exploring what's here. Either way, the product returned zero results.

### What it needs to communicate

1. **What happened.** "No results for X" — mirror the query back. The user should see their own input reflected so they can judge whether to refine.
2. **That data exists, just not matching this filter.** The user should not confuse "no matches" with "this area is empty." If there are 200 items and zero match the filter, say so — "0 of 200 match."
3. **How to recover.** Clear the filter. Broaden the search. Check spelling. Try a different field.

### What good looks like

- Headline includes the user's search term or filter criteria. "No results for 'acme'" — not "No results."
- A clear way to reset: "Clear filters," "Show all items." One click, not re-navigate.
- If you can suggest why zero results (common misspelling, filter too narrow), do so. "No results for 'acm' — did you mean 'acme'?"
- When counts are available, show the total: "0 of 347 items match your filters."

**Example (good):** A contacts list filtered by tag.
> **No contacts tagged "VIP"**
> 3 other tags have contacts. [Clear filter]

### What bad looks like

- "No results." Period. No mention of what was searched for, no way to clear, no suggestion. The user is stranded.
- Showing the create action ("Add a new item") when the user was searching, not creating. They want to find, not make.
- Hiding the active filter so the user doesn't realize they're in a filtered view. This happens when filter state is in the URL but not visually indicated — the user sees "nothing here" and thinks the area is empty.
- "Try a different search" with no ability to clear the current one from this screen.

### Counter-example

**Not filtered:** The user opens the contacts page for the first time and sees zero contacts. No filter is active — this is first-run, not filtered. The absence of a filter is the distinguishing factor.

### How to tell it apart

Filtered empty always has an active constraint — search query, filter selection, date range, tag. If any of these are active and the result set is zero, it's filtered empty. The treatment should always reflect the constraint and offer to remove it. If no constraint is active, it's one of the other four types.

---

## 4. Error empty

### The user's situation

They navigated to an area that should have content, but something failed — the API errored, the network dropped, the service timed out, the data source is unavailable. The user may or may not know something went wrong. From their perspective, they see nothing, and they don't know if "nothing" is accurate or broken.

### What it needs to communicate

1. **That something went wrong.** Clearly, without burying the lede. The user should not mistake an error for a legitimately empty area. "We couldn't load your tasks" is different from "You have no tasks."
2. **That it's not the user's fault.** Unless it actually is (malformed input, expired session). Most errors are system-side; the copy should reflect that. "We" failed, not "you" failed.
3. **How to recover.** Retry, refresh, check connection, contact support — whatever is appropriate for the error type. If the error is transient, offer retry. If it's persistent, offer an escalation path.

### What good looks like

- Headline states the failure clearly: "Couldn't load messages." Not "Something went wrong" (too vague) or "Error 500" (too technical).
- If the system knows what failed, say it. "The messages service isn't responding" is more useful than "An error occurred."
- A retry button if the error might be transient. Automatic retry with a timeout if the product knows the error class.
- No blame. "We couldn't load this" — not "Failed to fetch data" or "Request failed."

**Example (good):** A dashboard that failed to load analytics.
> **Couldn't load your analytics.**
> This usually resolves in a few minutes.
> [Try again]

### What bad looks like

- A blank screen with no indication that an error occurred. The user thinks the area is empty and leaves.
- "Something went wrong. Please try again later." — the most common default. It says nothing about what, nothing about when "later" is, nothing about what to do now.
- Technical error messages: "Error: ECONNREFUSED," "500 Internal Server Error," "TypeError: Cannot read property 'map' of undefined." These leak implementation.
- Apologizing instead of explaining: "Sorry! We're having trouble." The user doesn't need an apology; they need a recovery path.
- Showing a broken layout — half-rendered components, missing sections, skeleton screens that never resolve. If the data failed to load, show the error state, not a zombie loading state.

### Counter-example

**Not error empty:** The user types a search query and gets zero results. That's filtered empty, even if it feels like "nothing happened." The distinction: was there a constraint the user applied (filtered), or did the system fail to produce results it should have (error)?

### How to tell it apart

Error empty is the only type caused by system failure, not by the state of the user's data. The signal is in the response — an HTTP error, a timeout, a caught exception, a failed health check. If the API succeeded and returned zero items, it's one of the other types. If the API failed, it's error empty. Products that don't distinguish between "success with empty data" and "failure" will conflate error empty with first-run or cleared, which is the worst possible confusion — the user thinks they have nothing when they actually have plenty that failed to load.

---

## 5. Permissions empty

### The user's situation

They can see that an area exists — a navigation item, a container, a tab — but they don't have access to the contents. They might be a viewer in an editor-only area, a free user seeing a paid feature, or a team member without the right role.

### What it needs to communicate

1. **That content exists, they just can't see it.** The user should not think the area is empty. "You don't have access to reports" is different from "No reports."
2. **Why.** Role, plan, team settings — whatever determines access. Be specific without being technical: "Reports are available on the Pro plan" is better than "Insufficient permissions."
3. **What to do about it.** Request access, upgrade, contact an admin, learn more. Give the user a path, not a wall.

### What good looks like

- Headline states the access situation clearly: "You don't have access to reports."
- A reason: "Reports are available on the Team plan" or "Ask your workspace admin for access."
- An action that moves toward resolution: "Upgrade," "Request access," "Learn about plans."
- The tone is matter-of-fact, not gatekeeping. The user should not feel punished or teased.

**Example (good):** A free user seeing a premium feature.
> **Reports are on the Team plan.**
> You can see individual metrics in your dashboard.
> [See plans]

### What bad looks like

- A blank area with no indication that content exists behind the permission boundary. The user thinks the feature is empty or broken.
- "You don't have permission." Full stop. No explanation of why, no path to resolution.
- Teasing the content — showing blurred previews, titles without bodies, counts without access. This works for marketing pages; inside the product, it feels hostile.
- "Contact your administrator." With no indication of who that is, or in a product where the user might be the administrator. Always check.
- Conflating plan restrictions with role restrictions. "Upgrade to access" when the issue is that the user's workspace role doesn't allow it — no amount of upgrading will help, and the suggestion erodes trust.

### Counter-example

**Not permissions:** A user opens an area and sees zero items because they haven't created any. They have full access — the area is just empty. That's first-run, not permissions. The distinction: does the user have the ability to populate this area? If yes, it's first-run. If no (because permissions prevent it), it's permissions empty.

### How to tell it apart

Permissions empty requires a check: does the current user have the rights to create, view, or manage content in this area? If not, permissions empty. The trickiest case is when the user has partial permissions — they can view but not create, or they can see the list but not the details. Partial-permission states often need hybrid treatment: show what's available, explain what isn't, offer the path to full access.

---

## When a single empty state is two types

Some empty states can be multiple types simultaneously, or the system can't tell which type applies. Common cases:

- **First-run vs. cleared:** System doesn't track whether the user ever had items. Default to first-run treatment — orientation is never wasted.
- **Filtered vs. first-run:** User has a filter active but also has never created anything. Lead with the filter ("No results for 'acme'") and secondarily note the empty state ("You don't have any items yet").
- **Error vs. first-run:** System failure returns empty data instead of an error code. This is a bug — fix it. The user should never see "Create your first item" when their 50 items failed to load.
- **Permissions vs. first-run:** User can't create items due to their role. This is permissions empty, even if the area has never had items. Lead with the permissions explanation.

When in doubt, the type that provides more context wins. Permissions > Error > Filtered > Cleared > First-run, roughly in order of specificity.
