# COPY-PATTERNS.md

How to write good error messages. Organized by error surface, with patterns, anti-patterns, and the thinking behind each. This is the reference to reach for when drafting replacement copy in the finalize step.

## The colleague test

Before writing any error message, apply this test: if a colleague were sitting next to the user when the error happened, what would the colleague say?

They wouldn't say "Something went wrong." They'd look at the screen and say "Oh, the save didn't go through — hit save again, your stuff is still there."

They wouldn't say "Invalid input." They'd say "That field wants an email address — you're missing the @ part."

They wouldn't say "Error 403." They'd say "You don't have access to that. Ask Sarah to add you to the project."

The colleague has three advantages over most error messages: they can see what the user is trying to do, they know what went wrong, and they know what to try next. The system has all three of these too — it just doesn't use them. Good error copy closes this gap.

## Structure of an error message

Every error message, regardless of surface, has up to three parts:

1. **What happened** — the diagnosis. One sentence. Specific enough that the user doesn't need to re-read it.
2. **What to do** — the recovery. One action, or at most two alternatives. A button is better than a sentence.
3. **What if that doesn't work** — the escalation. Only when the primary recovery might not help. A link to support, a status page, or a fallback path.

Not every error needs all three. A validation error might only need part 1 (the field is highlighted, the fix is obvious). A major outage needs all three. But the structure is always: diagnosis, recovery, escalation.

**Bad structure:**
> "Something went wrong. Please try again later. If the problem persists, contact support."

This has three parts, but none of them carry information. "Something went wrong" is an empty diagnosis. "Try again later" is a vague recovery. "Contact support" has no link, no detail, no way for support to reproduce the issue.

**Good structure:**
> "We couldn't save your project — our servers aren't responding. [Retry now] — your changes are still here. If this keeps happening, check our [status page]."

Diagnosis (servers aren't responding), recovery (retry button, changes preserved), escalation (status page).

## Patterns by surface

### Toast errors

Toasts are transient — they appear and disappear. This constrains what they can communicate.

**Good for:** Retriable errors, informational errors, lightweight confirmations that something didn't work.

**Bad for:** Anything requiring the user to take action on the toast itself, anything with detail the user needs to remember, anything critical.

**Patterns:**
- Keep it to one sentence. If you need more, the error shouldn't be a toast.
- Include an action button when possible — [Retry], [Undo], [View details].
- Don't auto-dismiss toasts that require action. If there's a button, the toast stays until dismissed.
- If the error will resolve itself (automatic retry, reconnection), say so. "Reconnecting..." with a spinner is better than "Connection lost" that disappears.

**Anti-patterns:**
- Toast that says "Error!" with no detail and auto-dismisses in 3 seconds. The user saw red, felt anxiety, and learned nothing.
- Toast that contains a paragraph of text. By the time the user reads it, it's gone.
- Toast that says "Please try again" without a retry button. The user has to figure out what "try again" means (reload the page? re-click? re-submit the form?).
- Toast for an error that actually needs inline treatment. A validation error in a toast, detached from the field it refers to, is worse than useless.

### Error pages (404, 500, permission errors)

The user's entire screen is the error. This is the product's most public failure — it's the moment that gets screenshotted and posted.

**Patterns:**
- **404 — thing not found.** Acknowledge what they were looking for if possible. Offer search, offer the parent page, offer the homepage. "This page doesn't exist" is better than "404 Not Found." "We couldn't find a project called 'acme-prod'" is better still.
- **500 — server error.** Be honest. "Our servers are having trouble" is better than "Something went wrong." If you have a status page, link it. If you know the issue is temporary, say so. If the user had unsaved work, address it.
- **403 — no permission.** Tell the user why they can't access this (permission, account tier, region) and who can help. "You don't have access to this workspace. Ask [workspace admin] to invite you." Never just say "Forbidden."
- **Maintenance.** Different from an error — the tone should be informational, not apologetic. "We're upgrading our infrastructure until 3pm UTC. Here's what's affected."

**Anti-patterns:**
- The clever 404 page that prioritizes being funny over being useful. If the user can't find what they're looking for, a joke is salt in the wound. One link back to somewhere useful outweighs any amount of wit.
- "An error occurred" as the entire content of a full-page error. The page is empty. The user is stranded. There is no link, no action, no information.
- Error pages that lose the navigation. The user was in the product; now they're on a blank white page with no way back except the browser's back button.
- Error pages that show a stack trace in production. This is a security issue and a trust issue simultaneously.

### Empty error states

The user loaded a page and the data fetch failed. The screen is empty, or shows a skeleton that never resolves. This is different from a proper error page — it's a partial failure within an otherwise-working product.

**Patterns:**
- Show the page's chrome (nav, sidebar, header) and put the error in the content area. Don't blank the entire screen for a content-area failure.
- Distinguish between "no data" (empty state) and "couldn't load data" (error state). These need different copy and different actions. An empty state says "nothing here yet, here's how to add something." An error state says "there should be things here but we couldn't load them."
- Offer a retry that fetches just the failed content, not a full page reload.
- If stale data is available (from cache), show it with a banner: "Showing data from 2 hours ago. Couldn't get the latest. [Retry]"

**Anti-patterns:**
- Eternal loading spinner with no timeout. The user waits 30 seconds, then manually refreshes, then waits 30 more seconds. A loading state must have a failure threshold.
- Blank white content area with no explanation. The user doesn't know if it's still loading, if there's nothing there, or if something broke.
- "No results" when the real answer is "search failed." These are different things. "No results" means the query ran and found nothing. "Search failed" means the query didn't run. Conflating them misleads the user.

### Inline validation messages

See VALIDATION-MESSAGES.md for the full treatment. Key copy patterns here:

**Patterns:**
- Name the field. Name the constraint. Show or imply the fix. "Password needs at least 8 characters" does all three.
- Use the present tense. "This field is required" not "This field was left blank."
- Write for scanning. The user is looking at a form with possibly multiple errors. Each message should be self-contained and short enough to read in a glance.
- Place the message adjacent to the field. Below the field is standard. Above the field is unusual and forces the user's eyes to backtrack.

**Anti-patterns:**
- "Invalid value." For any field, for any reason. This is the "Something went wrong" of validation — it says nothing.
- "This field is required." Without identifying the field by name. When there are multiple errors, the user is jumping between fields and needs each message to be self-locating.
- "Error in form." As a single message at the top of the page, with no per-field indicators. The user has to play detective.
- Red field borders with no message text. The user knows something is wrong but not what.

## Voice considerations

### Don't apologize by default

"Sorry, something went wrong" is the default error pattern in many products. "Sorry" is meaningful only if the product's voice actually apologizes. In most products, "sorry" is filler — it takes up space without adding information or warmth.

Replace "sorry" with usefulness. "Sorry, we couldn't save" becomes "We couldn't save — [Retry]." The user doesn't need an apology; they need their data saved.

Exception: when the product genuinely caused a significant disruption and the voice supports it. "We're sorry — an issue on our end deleted your draft. We're working to recover it." Here, "sorry" is earned because the product broke something important.

### Don't be aggressively casual

"Whoops! Something went sideways!" is not better than "Something went wrong." Forced casualness in an error state can feel dismissive — the user is frustrated, and a wacky tone suggests the product doesn't take the problem seriously.

Match the gravity of the error to the tone. A validation nudge can be light. A data loss error should be serious. A payment failure should be professional.

### Don't use exclamation marks in errors

Unless the product's voice is genuinely exclamatory (which is rare and should be documented in CONTEXT.md), exclamation marks in errors read as either panicked or falsely cheerful:

- "Error!" — panicked
- "Oops!" — falsely cheerful
- "Try again!" — weirdly enthusiastic about failure

Periods and question marks are almost always the right punctuation for errors.

### Use the product's vocabulary

If the product calls them "workspaces," don't say "your account area" in the error. If the product calls them "team members," don't say "users" in the error. Errors are part of the product, not a separate system with its own vocabulary.

## Anti-patterns index

These patterns appear so frequently that they deserve to be called out by name:

**The blame shift.** The system failed but the error says "Please check your input." The user didn't do anything wrong, but the message implies they did. Root cause: catch-all error handling that routes all failures through the same user-facing message.

**The empty apology.** "Sorry, something went wrong." Neither the sorry nor the diagnosis carries information. Root cause: error copy written as a formality, not as communication.

**The phantom retry.** "Please try again" without a retry button, without preserving the user's state, or for an error where retrying won't help. Root cause: "try again" used as a default recovery for all error types without considering whether retry is the right action.

**The vanishing detail.** A toast or notification that contains important information but auto-dismisses before the user can read or act on it. Root cause: all toasts on the same timer regardless of content.

**The jargon leak.** "Error: UNIQUE constraint failed: users.email" shown to a user. Root cause: error message from the database or framework passed directly to the UI without translation.

**The dead end.** Error message with no action, no link, no path forward. The user's only option is the browser's back button. Root cause: error handling that focuses on logging the error for developers and forgets the user still needs to go somewhere.

**The false reassurance.** "Don't worry, everything is fine!" when it clearly isn't, or "This should only take a moment" for something that takes minutes. Root cause: discomfort with delivering bad news.

**The double error.** The user hits an error, tries to recover, and hits a second error about the recovery. "Your session expired" -> clicks login -> "Login service unavailable." Root cause: error recovery paths that aren't tested under the same failure conditions.
