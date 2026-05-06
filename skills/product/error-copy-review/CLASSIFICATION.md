# CLASSIFICATION.md

The four error categories. Every error in a product falls into one of these. The category determines the posture — what the message needs to communicate, what tone it should take, and what action it should surface. Misclassifying an error is the root cause of most bad error copy: a blocking error treated as user-fixable blames the user for a system problem; a user-fixable error treated as informational buries the fix.

## The four categories

### User-fixable

**The user's situation:** They did something the system can't process — mistyped an email, left a required field blank, chose a username that's taken, uploaded a file that's too large. The system knows exactly what's wrong and exactly how to fix it.

**What the error needs to communicate:** What specific thing is wrong, and how to fix it right now. Not "invalid" — what's invalid and what would be valid. The user should be able to read the message and take the correct action without guessing.

**The right posture: specificity.** The message must name the field, name the constraint, and either show or imply the fix. The more specific, the faster the user recovers.

Good:
- "Email needs an @ and a domain — like name@example.com"
- "Password must be at least 8 characters. You're at 6."
- "That username is taken. Try adding a number or pick from: ..."
- "This file is 12 MB. The limit is 10 MB."

Bad:
- "Invalid email" — invalid how?
- "Password does not meet requirements" — which requirements?
- "Username unavailable" — is it taken, reserved, or forbidden?
- "File too large" — how large is it, and what's the limit?

**Edge cases:**

- **Ambiguous field ownership.** A form where the "company name" field fails uniqueness validation — is this user-fixable? The user can change the name, but they may not want to. The copy needs to acknowledge this: "A company with that name already exists. If this is your company, contact support to claim it." The fix might not be in the form.
- **Cascading validation.** Field A's value makes field B's value invalid. Marking only field B confuses the user. Acknowledge the dependency: "Because you selected monthly billing, the start date must be in the current month."
- **Constraints the user can't see.** "That slug is reserved" — reserved by whom? The user didn't know there was a reserved list. Specificity means exposing the constraint, not just the violation.

---

### Retriable

**The user's situation:** They did something reasonable and it didn't work — but not because of anything they did. The network hiccupped. A service timed out. A concurrent edit caused a conflict. Trying the same thing again in a few seconds has a real chance of succeeding.

**What the error needs to communicate:** It didn't work this time, it's not your fault, and here's a way to try again without repeating your work.

**The right posture: retry action.** Don't just say "try again" — provide a button or link that retries. If the user filled out a form, preserve their input. If the operation was a save, make sure the retry doesn't duplicate data. The cost of retrying should be near zero.

Good:
- "Couldn't save your changes — the connection dropped. [Retry] Your edits are still here."
- "That request timed out. [Try again]" (with the form still populated)
- "The server took too long to respond. [Retry] or check your connection."

Bad:
- "An error occurred. Please try again later." — when is "later"? And where's the retry button?
- "Request failed." — why? And what now?
- "Network error" — technically true, humanly useless.
- "Something went wrong. Please try again." — the user already knows something went wrong; they're looking at the error.

**Edge cases:**

- **Retry that won't help.** Some errors look retriable but aren't — a 429 rate limit needs a wait, not an immediate retry. If the system knows a retry needs a delay, say so: "Too many requests. You can try again in 30 seconds." Don't offer an instant retry button that will fail again.
- **Partially completed operations.** The request went through but the confirmation didn't come back. A naive retry might duplicate the action. If the system can't tell whether it succeeded, say so: "We're not sure if that went through. Check [location] before trying again."
- **Retry with stale context.** A conflict error (409) means the data changed since the user loaded it. Retrying with the same data will fail again. The user needs to see what changed: "Someone else edited this while you were working. Here's their version — review and save again."

---

### Blocking

**The user's situation:** Something is genuinely broken and they can't do what they came to do. The service is down, their account is suspended, the resource doesn't exist, they don't have permission. No amount of retrying or editing their input will help.

**What the error needs to communicate:** What's actually happening, that it's not their fault (when it isn't), and what alternatives exist — even if the alternative is "wait" or "contact someone."

**The right posture: honesty.** Don't minimize a real outage. Don't pretend the user can fix it. Don't offer a retry button for something that won't resolve by retrying. Say what's true, and give the user whatever you can — an estimated timeframe, a status page link, a support contact, an alternative route.

Good:
- "Our payment processor is down right now. We're working on it. Check [status page] for updates, or we'll email you when it's back."
- "You don't have permission to view this project. Ask [owner name] to invite you, or go back to your dashboard."
- "This page doesn't exist. It may have been deleted or moved. [Go to dashboard]"
- "Your account is suspended. Contact support@example.com to resolve this."

Bad:
- "500 Internal Server Error" — the user didn't ask for a status code.
- "Something went wrong. Please try again later." — for a known outage, this is dishonest.
- "Access denied." — denied by whom? And what do I do about it?
- "Page not found." — with no link anywhere else.

**Edge cases:**

- **Partial outage.** The product mostly works but one feature is down. Don't show a full-screen error page — scope the error to the broken feature and let the user continue using what works. "Notifications aren't loading right now. Everything else is working fine."
- **Permissions that could change.** "You don't have permission" is blocking now but fixable if the user contacts the right person. Always name who can grant access, if the system knows.
- **Scheduled maintenance.** This is blocking, but the user shouldn't feel like something is broken. "We're doing planned maintenance until 3pm UTC. Here's what's affected and what still works." Different tone than an outage.
- **Account-level blocks.** Suspension, billing issues, expired trials. The copy needs to be firm but not punitive. The user is probably already frustrated. Tell them exactly what to do and who to contact.

---

### Informational

**The user's situation:** Something didn't work perfectly, but it's not critical to what they're doing right now. A background sync failed. A non-essential integration is disconnected. An optional feature hit a limit. The user can keep working.

**What the error needs to communicate:** What happened, that it's not urgent, and where to deal with it if they want to — but don't interrupt their flow.

**The right posture: quietness.** Don't use modals. Don't use alarming colors. Don't stop what the user is doing. A subtle toast, a status indicator, a badge — something the user can notice without being forced to engage with.

Good:
- (Subtle toast) "Background sync paused — will retry automatically."
- (Status badge) "Slack integration disconnected. Reconnect in Settings."
- (Inline note) "3 contacts couldn't be imported. See details."
- (Non-blocking banner) "You've used 90% of your storage. Upgrade anytime in billing."

Bad:
- (Modal) "Sync error! Something went wrong with your sync." — a modal for a background process is violence.
- (Red banner, top of page) "INTEGRATION ERROR" — for a non-essential Slack notification.
- (Toast that disappears in 3 seconds) "3 contacts failed to import" — if there are details the user might need, don't flash them and vanish.

**Edge cases:**

- **Informational that escalates.** A background sync failure is informational the first time. After 24 hours of failures, it's blocking — data is stale and the user doesn't know it. Design escalation: if the informational error persists, it should promote itself to a more visible state.
- **Informational about data the user cares about.** "3 contacts couldn't be imported" is informational in tone but critical to a user who needs all 3. The message needs to make it easy to find the details without forcing everyone through them.
- **Quiet but not hidden.** Informational doesn't mean invisible. If the user looks for the status, they should find it. A log, a status page, an expandable detail. The initial notification is quiet; the detail is accessible.

## When categories blur

Real errors don't always fit neatly. Some guidelines for the gray areas:

**User-fixable vs. retriable:** The user typed a valid email but the verification service is down, so the system can't confirm it. Is this user-fixable (they could try a different email) or retriable (the service will come back)? Default to retriable — the user did nothing wrong. Accept the email and verify it asynchronously, or tell the user to try again shortly.

**Retriable vs. blocking:** A network timeout might resolve itself (retriable) or might indicate a real outage (blocking). Offer one retry. If it fails again, escalate the message: "This still isn't working. It might be a larger issue — check [status page] or try again later."

**Blocking vs. informational:** The user's Slack integration is disconnected. Blocking if they're trying to use it right now; informational if they're doing something else. Context determines the category. If the user just clicked "Send to Slack," it's blocking. If they're editing a document, it's informational.

**Multiple categories at once:** A form submission fails because one field is invalid (user-fixable) and the server also returned a 500 for the save attempt (blocking). Handle both: show the field error and tell the user the save failed independently. Don't merge them into one vague message.
