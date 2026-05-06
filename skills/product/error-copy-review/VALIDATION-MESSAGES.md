# VALIDATION-MESSAGES.md

Inline form validation is its own discipline. It's the most frequent error surface in most products — users encounter validation messages far more often than 500 pages or outage banners. And because validation messages are small and repetitive, they're the most likely to be left as defaults.

This file covers the craft of validation messages specifically. For general error copy patterns, see COPY-PATTERNS.md. For how validation errors fit into the broader error taxonomy, see CLASSIFICATION.md (they're almost always user-fixable).

## The two levels

### Field-level errors

Attached to a specific field. The user can look at the field, read the message, and fix the problem without thinking about the rest of the form.

**This is the right level for:** constraint violations on a single field (format, length, required, uniqueness, range), and any error where the fix is changing one field's value.

**How to write them:**
- Name the constraint, not just the violation. "Password must be at least 8 characters" (constraint) vs. "Invalid password" (violation without explanation).
- Show what the user has vs. what's needed, when practical. "Password must be at least 8 characters — you've entered 5" closes the gap between the user's mental model and the system's requirement.
- Be specific about format expectations. "Phone number: use digits only, like 555-123-4567" is better than "Invalid phone number."
- Keep them to one line. The user is scanning, not reading. If a field's validation requires a paragraph to explain, the constraint might be too complex.

### Form-level errors

Not attached to any single field. Either the error involves the combination of multiple fields, or the form submission itself failed for a reason unrelated to individual field values.

**This is the right level for:** cross-field validation ("end date must be after start date"), server-side validation that can't be checked per-field ("this combination of settings isn't supported"), and submission failures (network, auth, server errors).

**How to write them:**
- Appear at the top of the form, above the submit button, or in a summary block.
- Reference the specific fields involved, by name. "End date must be after start date" tells the user which two fields to look at. "Invalid dates" doesn't.
- If the form-level error coexists with field-level errors, make the relationship clear. "Fix the highlighted fields below, then try again" bridges from the summary to the per-field messages.
- If the form submission failed for a server reason, don't present it as a validation error. "We couldn't save this right now — try again" is a submission failure. "Please correct the errors below" is validation. They look different and mean different things.

## Timing

When to show validation messages is as important as what they say. Bad timing makes correct messages feel hostile.

### On blur (when the user leaves the field)

**Best for:** Format validation (email, URL, phone), constraint checks that can run on a single field's value, and any validation where the user is done entering the value.

**Why it works:** The user has finished typing and moved on. If the field is wrong, this is the natural moment to tell them — before they've gone further and built more context that would be lost.

**Why it can fail:** Overeager blur validation on fields where the intermediate state looks invalid. An email field that shows "Invalid email" while the user is still typing the domain. A date field that errors when only the month is filled in. If the field is commonly filled in stages, delay validation until the user has had a chance to complete it.

### On submit

**Best for:** Cross-field validation, server-side validation, and any check that requires the full form context.

**Also the fallback for:** Any validation that wasn't caught on blur, because the field wasn't touched, was auto-filled, or the blur handler didn't fire.

**How to handle it well:**
- Scroll to the first error if the form is long.
- Focus the first invalid field so the user can immediately start fixing it.
- Preserve all entered data. Clearing the form on validation failure is unforgivable — the user did work, and the product destroyed it.
- If multiple fields have errors, show all of them at once. Revealing them one at a time ("fix this... now fix this... now fix this...") trains the user to expect more hidden problems and erodes trust.

### Real-time (as the user types)

**Best for:** Very specific constraints where immediate feedback is genuinely helpful: password strength meters, character counters, username availability.

**Dangerous for:** Almost everything else. Real-time validation that shows errors while the user is mid-keystroke is hostile. Typing "j" in an email field and immediately seeing "Invalid email" is the system shouting at the user before they've had a chance to finish a thought.

**The rule:** Real-time validation should show progress toward validity, not errors about incompleteness. A character counter counting down is helpful. A red "too short" message that appears on the first keystroke is not.

**The exception:** Real-time validation that clears a previously-shown error is always welcome. If the user fixed the email and it now passes format validation, remove the error immediately — don't wait for blur or submit.

## Constraint types

Different types of constraints need different copy approaches.

### Format constraints (email, URL, phone, date)

The user entered something, but it doesn't match the expected pattern.

**Good pattern:** Show the expected format as an example. "Enter a valid URL — like https://example.com" is better than "Invalid URL." The example does the work that the word "valid" is too lazy to do.

**Common fields:**

- **Email:** "Needs an @ and a domain — like name@company.com"
  - Don't reject valid but unusual emails (plus addressing, long TLDs, hyphens). If your regex rejects `user+tag@example.com`, the bug is the regex, not the user.
- **URL:** "Enter the full URL, starting with https:// — like https://example.com"
  - Consider accepting URLs without the protocol and adding it silently. If the user types "example.com" and you can figure out what they mean, don't make them type "https://".
- **Phone:** "Use digits only — like 555-123-4567" or "Include your country code for international numbers — like +1 555-123-4567"
  - Phone number formats vary wildly by country. Be specific about what format you want, or be very permissive about what you accept. Never reject a valid international number because your regex expects US format.
- **Date:** "Use MM/DD/YYYY — like 03/15/2025"
  - Date formats differ by locale. If you require a specific format, show it in the placeholder and the error. Better yet, use a date picker and make the format moot.

### Length constraints (min, max, exact)

The value is too short, too long, or not the right length.

**Good pattern:** Show the current length and the requirement. "Company name must be 3-50 characters. You've entered 2." The user can do the math, but shouldn't have to.

**Anti-pattern:** "Input too long." How long is it? What's the max? The user is about to start deleting characters one at a time, re-submitting each time, playing a guessing game the system could end in one message.

### Uniqueness constraints (username, email, slug)

The value is already taken.

**Good pattern:** Acknowledge the conflict and offer alternatives. "That username is taken. How about sarah-m, sarahm2, or sarah-miller?" Generating suggestions is easy for the system and hard for the frustrated user who just tried three usernames in a row.

**Anti-pattern:** "Username unavailable." No suggestions, no explanation of whether it's taken by another user or reserved by the system. The user is now going to try random variations until one works.

**Edge case:** The user is trying to register with an email that already has an account. This isn't a uniqueness error — it's a "you already have an account" situation. "This email is already registered. Did you mean to sign in?" with a link to sign-in. Don't make the user go find the sign-in page themselves.

### Required fields

The field was left blank.

**Good pattern:** "Project name is required" — names the field, states the constraint. Simple. If the field is required and the reason isn't obvious, briefly explain why: "Phone number is required for two-factor authentication."

**Anti-pattern:** 
- "This field is required." Generic, doesn't name the field, and when multiple fields are required, the user is playing match-the-message-to-the-field.
- Red asterisks with no explanation. The user doesn't know what the asterisk means until they submit and get an error. Explain the convention near the top of the form, or better, mark optional fields instead of required ones (since most fields in most forms are required).
- Required fields that aren't marked at all until submission, when suddenly three of them turn red. If a field is required, the user should know before they try to submit.

### Range and logical constraints

The value is outside an allowed range, or logically invalid given other values.

**Good pattern:** Show the allowed range and where the user's value falls. "Quantity must be between 1 and 100. You entered 0." For cross-field constraints: "End date (March 1) must be after start date (March 15). Update one of the dates."

**Anti-pattern:** "Invalid value." For a number field. Which direction is it invalid? Is it too high, too low, negative, zero, or non-numeric? The user is reduced to trial and error.

## Anti-patterns specific to validation

### Clearing the form on error

The user filled out 12 fields, hit submit, and one field is invalid. The form reloads blank. The user now has to redo 11 correct fields to fix one wrong one. This is the single most damaging validation anti-pattern. Preserve everything. Always.

### Hiding errors behind a click

"There are errors in your form. Click here to see them." The user can already see that something went wrong — the submit didn't work. Making them click to see the errors is adding a step to an already frustrating flow.

### Disabling submit without explanation

The submit button is grayed out. Why? The user can't tell which field is preventing submission. Either show the validation errors inline (preferred) or show a tooltip on the disabled button explaining what's needed.

### Aggressive real-time validation

Showing "Invalid email" as the user types "j" in the email field. The user hasn't finished typing. Wait for blur, or at least wait for a significant pause. Real-time validation should be reserved for progressive indicators (password strength, character count, availability checks that the user is waiting for).

### Generic catch-all

Every field's error is "Invalid value" or "This field has an error." The system knows exactly what's wrong — the value is too long, the format doesn't match, the email is taken — but the message hides all of that behind a generic label. This is almost always a sign that error messages are defined in the validation schema as an afterthought.

### Error messages that conflict with placeholders

Placeholder: "e.g., https://example.com". Error: "Must start with http:// or https://". The user entered "example.com" because the placeholder seemed to suggest that was fine. If the placeholder shows a specific format, the validation must match. If the validation requires a protocol, the placeholder should show one.

## The validation stack

For any form, validate in this order. Each layer catches what the previous one missed.

1. **HTML attributes** — `required`, `type="email"`, `maxlength`, `pattern`. Caught by the browser, zero JavaScript needed. Provides a baseline of constraint communication.
2. **Client-side validation** — runs on blur or submit, before the network call. Catches format errors, length errors, required fields, and any constraint the client can check. This is where most validation messages live.
3. **Server-side validation** — runs after submission. Catches everything the client can't: uniqueness, authorization, business rules, cross-entity constraints. Server-side messages need the same copy treatment as client-side ones — they're still user-facing.
4. **Database/system constraints** — the last line. If an error makes it to here and surfaces a raw constraint violation to the user, it's a bug. These should always be translated to human messages by the server layer.

Every layer should produce user-facing messages that match the patterns in this file. The deeper the layer, the more likely the message is a developer-facing default that leaked through.
