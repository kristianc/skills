# SCORING-RUBRIC.md

The four axes for scoring error copy. Every error message gets rated 1-5 on each axis. Anything below 3 on any axis is a candidate for rewriting. The axes are independent — an error can score 5 on clarity and 1 on recovery, or 5 on voice and 1 on blame.

The point of scoring is triage, not perfection. A product with 200 error messages needs to know which 30 to fix first. The scores tell you where the worst offenders are.

## Axis 1: Clarity

Does the message tell the user what happened?

**1 — Opaque.** The message communicates nothing about the problem. The user has no more information after reading it than before.

- "Something went wrong."
- "Error."
- "An unexpected error occurred."
- "Oops!"
- "Request failed."

The user's internal response: "What happened?" They have no information to act on.

**2 — Category only.** The message communicates the general type of problem but nothing specific. The user knows the neighborhood but not the address.

- "Network error."
- "Validation error."
- "Permission denied."
- "Server error."

The user knows roughly where the problem is but still can't diagnose or fix it.

**3 — Specific but incomplete.** The message identifies the problem but leaves out information the user needs. Often these messages name the what but not the why or how.

- "That email is already in use." (What should I do about it?)
- "File upload failed." (Why? Size? Format? Connection?)
- "Your session has expired." (Okay, but what happened to my work?)

The user understands the problem but has to guess at the context or the fix.

**4 — Specific and actionable.** The message explains what happened and implies or states what to do. The user can act immediately.

- "That email is already registered. Sign in instead, or use a different email."
- "The file is 25 MB — the limit is 10 MB. Compress it or choose a smaller file."
- "Your session expired. Your draft has been saved — sign in to continue."

The user reads it once and knows what to do.

**5 — Contextually complete.** The message uses everything the system knows to give the user full context. It explains what happened, why, and what to do — including edge cases and alternatives.

- "That email is registered to a Google SSO account. Sign in with Google, or use a different email to create a password-based account."
- "The upload failed because your connection dropped at 80%. The partial file has been saved — you can resume when you're back online."
- "Your session expired after 30 minutes of inactivity. We saved your draft at 2:34 PM — sign in to pick up where you left off."

The user feels like the system understood their situation, not just the error code.

### Scoring clarity — edge cases

- **Intentionally vague errors.** Sometimes you must be vague for security (login failures shouldn't reveal whether the email exists). Score these based on whether they're as clear as they can be within the constraint. "That email or password is incorrect" is a 4 for a login error — it's as specific as security allows.
- **Technical errors the system can't diagnose.** A generic catch-all for truly unknown errors can't score above 3 on clarity — but it can be a good 3. "We hit an unexpected problem. If this keeps happening, contact support with error code XYZ" is better than "Something went wrong."
- **Error codes.** Including an error code alongside human copy is fine and can help support teams. Including only an error code is a 1.

---

## Axis 2: Recovery

Does the message tell the user what to do next?

**1 — Dead end.** The message offers no path forward. The user is stranded.

- "Something went wrong." (Full stop. No button, no link, no suggestion.)
- "Error 500." (Is there literally anything I can do?)
- A blank screen after a failed load, with no explanation or action.
- An error modal with only an "OK" button that dismisses the error and leaves the user where they were.

**2 — Vague suggestion.** The message gestures at a recovery but doesn't make it concrete or actionable.

- "Please try again later." (When is later? How do I try again?)
- "Contact support." (Where? How? What do I tell them?)
- "Check your input and try again." (Which input? What's wrong with it?)

**3 — Generic action.** The message provides a real action, but it's a one-size-fits-all response rather than something tailored to the specific error.

- "Try again" button on every error, regardless of whether retrying will help.
- "Go back to dashboard" on every 404, even when the user could be pointed to a specific resource.
- "Refresh the page" — sometimes works, but the user is still guessing.

**4 — Specific action.** The message provides an action tailored to this error. The user knows exactly what to do and the action is one click or step away.

- "Your card was declined. Update your payment method." (with a link to billing)
- "[Retry]" button on a timeout error, with the user's data preserved.
- "This workspace was deleted by [admin]. Contact them to restore it, or create a new workspace."

**5 — Recovery built in.** The system doesn't just suggest recovery — it enables or performs it. The user's path back is frictionless.

- The form preserves all input and highlights only the field that needs changing.
- A failed save is automatically retried, with a message only if the retry also fails.
- A conflict error shows a diff of the two versions and lets the user merge.
- A session expiry saves the draft and restores it seamlessly after re-authentication.
- An upload failure resumes from where it left off.

### Scoring recovery — edge cases

- **Errors with no possible recovery.** Account banned, resource permanently deleted, regulatory restriction. These can still score 3-4 by being honest and giving the user the best available next step (contact info, documentation link, alternative path).
- **Multiple possible recoveries.** When the user could retry, edit, or escalate, offering all three as clear options is a 5. Offering only one when others exist is a 3.
- **Recovery that requires leaving the product.** "Contact your IT admin" is a real recovery path, but it's outside the product. Score it 3 — it's helpful but the product can't close the loop.

---

## Axis 3: Voice

Does the message sound like the product, or like the system?

Check CONTEXT.md for the product's voice. If there's no CONTEXT.md, evaluate against basic craft standards: does it sound like a human wrote it for another human?

**1 — System-speak.** The message sounds like it was generated by a framework, database, or HTTP library. No human voice.

- "Error: ETIMEDOUT"
- "Invalid input: field 'email' failed validation rule 'isEmail'"
- "Unprocessable Entity (422)"
- "null is not an object (evaluating 'response.data.user')"
- "Operation completed with errors."

**2 — Developer-speak.** A human wrote it, but a developer talking to themselves, not a user. Jargon, implementation details, or tone that assumes the reader knows how the system works.

- "The API returned a 429. Reduce request frequency."
- "WebSocket connection dropped. Reconnecting..."
- "Cache miss — fetching from origin."
- "Request payload exceeds maximum allowed size."

**3 — Neutral but generic.** Grammatically human, free of jargon, but could appear in any product. No personality, no voice, no awareness of the product's identity.

- "An error occurred. Please try again."
- "Unable to save your changes."
- "Your request could not be processed."
- "The file you uploaded is too large."

This is where most products plateau. It's not bad — it's bland. And bland accumulates.

**4 — In voice, with minor drift.** The message mostly sounds like the product, but occasionally slips into generic patterns, unnecessary formality, or filler.

- "We couldn't save that — try again in a moment." (Good, but "in a moment" is filler — when?)
- "Hmm, that file is too big. The max is 10 MB." (The "Hmm" may or may not match the product's voice.)
- "Sorry, we're having trouble connecting. Please try again." ("Sorry" and "Please" might be filler, depending on the voice.)

**5 — Fully in voice.** The message is indistinguishable from the product's best non-error copy. Same vocabulary, same tone, same level of directness. You could read it without knowing it was an error and still know which product it came from.

What this sounds like depends entirely on the product. A playful product's 5 sounds different from a financial product's 5. The test: read the error message next to the product's marketing copy, onboarding copy, and success states. Does it belong?

### Scoring voice — edge cases

- **Products without a defined voice.** If CONTEXT.md doesn't exist and the product has no discernible voice, score against "would a thoughtful colleague say this?" rather than against a brand guide. A 5 without a voice guide is "sounds like a helpful, knowledgeable human."
- **Voice vs. tone.** Voice is consistent; tone varies by context. A playful product can be serious in error copy without breaking voice. The question is whether the shift in tone feels deliberate or whether the error copy was written by someone who hadn't read the rest of the product.
- **Error messages in technical contexts.** A developer tools product might legitimately use "422 Unprocessable Entity" in its voice. The test is whether that's the product's vocabulary (in which case it's a 5) or a framework leak (in which case it's a 1). Context determines this.

---

## Axis 4: Blame

Does the message blame the user for the problem?

Error copy has a default direction: toward the user. "Invalid input." "You entered an incorrect password." "Your request failed." The subject is always you, and the verb is always something you did wrong. This axis measures how well the copy avoids unwarranted blame.

**1 — Accusatory.** The message directly blames the user, even when the fault might be the system's.

- "Invalid input." (For any error, including server failures.)
- "You entered an invalid email address." (Did I, or did your regex reject a valid one?)
- "Unauthorized. You are not allowed to perform this action." (In passive-aggressive legal tone.)
- "Wrong password." (Terse. Feels like a slap.)

**2 — Implicitly blaming.** The message doesn't directly accuse, but frames the error as the user's problem to solve, even when the system caused it or could prevent it.

- "Please check your input and try again." (After a server error.)
- "Make sure you entered a valid URL." (When the URL was fine but the server is down.)
- "Your session expired. Please log in again." (Why did it expire? Was any work lost?)

**3 — Neutral.** The message doesn't blame, but it doesn't take responsibility either. It describes the situation without assigning a subject.

- "The file could not be uploaded."
- "This action could not be completed."
- "An error occurred while saving."

Neutral is the minimum bar. It's not warm, but it's not hostile.

**4 — System takes responsibility when appropriate.** The message attributes the failure to the system when the system is at fault, and frames user errors as situations to resolve rather than mistakes to correct.

- "We couldn't reach our servers — try again in a moment."
- "That password doesn't match our records. Reset it or try again."
- "We hit a problem saving your changes. Your edits are safe — try saving again."

**5 — Blame is never misplaced; user errors are framed as collaboration.** The message never blames the user for system problems. When the user genuinely made an error, the copy frames it as "here's the situation and here's how to resolve it" rather than "you did this wrong."

- "Looks like that email needs an @ and a domain — something like name@company.com."
- "That password is too short — it needs at least 8 characters to keep your account secure."
- "Our servers are having trouble right now. We're looking into it. Your work has been saved locally."
- "This email is already registered. Want to sign in instead?"

The user never feels scolded, even when they did make a mistake. The product is on their side.

### Scoring blame — edge cases

- **User genuinely at fault.** The user entered "asdf" in a phone number field. It's still possible to score 5 — "That doesn't look like a phone number. Use digits, with an optional country code." The blame axis doesn't mean pretending the user didn't make a mistake. It means framing the mistake as a situation to resolve.
- **Security-sensitive contexts.** "Incorrect password" can't say "your password is close, try..." for security reasons. But it can avoid "Wrong password." and say "That password doesn't match. Try again or reset it." The constraint is real; the tone still matters.
- **System errors described passively.** "An error occurred" is neutral (3) — but if the system knows it was a server crash, passive voice hides useful information. "Our servers hit a problem" is more honest and scores higher on both clarity and blame.

## Using the scores

After scoring, sort candidates by their lowest individual score. An error that scores 5/5/5/1 (great clarity, recovery, voice, but accusatory) is as much a candidate as one that scores 1/1/1/5. The minimum score across axes determines urgency.

For a quick triage view:

| Priority | Condition |
|----------|-----------|
| Fix first | Any axis at 1 |
| Fix soon | Any axis at 2, none at 1 |
| Improve | Any axis at 3, none below |
| Okay for now | All axes at 4+ |

A product where every error scores 4+ on every axis has excellent error copy. Most products have a long tail of 1s and 2s hiding in catch blocks and validation schemas.
