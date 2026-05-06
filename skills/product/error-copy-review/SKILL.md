---
name: error-copy-review
description: Find every error message, toast, validation string, and failure state in a product and rewrite them in the product's voice with recovery actions. Use when the user wants to audit error copy, fix generic error messages, make validation helpful, or ensure every failure state tells the user what went wrong and what to do next.
---

# Error Copy Review

Find every error message in the product and rewrite it so it explains what happened, in the product's voice, with a path to recovery.

Error messages are the moments when the product's relationship with the user is most fragile. Generic errors ("Something went wrong") erode trust. Accusatory errors ("Invalid input") erode goodwill. Silent errors erode both. The goal is to make every failure state honest, helpful, and in voice.

## What counts as error copy

- **Validation messages** — inline field errors, form-level errors, constraint violations
- **API/network errors** — failed requests, timeouts, 4xx/5xx responses surfaced to the user
- **Toasts and banners** — transient error notifications
- **Error pages** — 404, 500, permission denied, not found
- **Empty error states** — screens that show nothing because a fetch failed
- **Console/log messages shown to users** — stack traces, raw error codes, or technical messages that leak through

## Process

### 1. Inventory

Use the Agent tool with `subagent_type=Explore` to find every error message in the codebase. Look for:

- Strings containing "error", "fail", "invalid", "wrong", "sorry", "oops", "try again", "went wrong", "unable to", "could not"
- Toast/notification calls with error severity
- Validation schemas and their message strings
- Error boundary components and their fallback renders
- HTTP error handlers and their user-facing messages
- `catch` blocks that surface messages to the UI

For each one, record:
- **Location** — file, component, line
- **Trigger** — what causes this error to appear
- **Current copy** — the exact text the user sees
- **Recovery offered** — what action, if any, the user is given (retry, go back, contact support, nothing)

### 2. Classify

Sort every error into one of these categories:

| Category | The user needs to know... | Example |
|----------|--------------------------|---------|
| **User-fixable** | What's wrong and how to fix it | "Email must include @" |
| **Retriable** | It failed but trying again might work | Network timeout |
| **Blocking** | It's broken and they can't proceed right now | Service outage |
| **Informational** | Something didn't work but it's not critical | Background sync failed |

The fix for each category is different. User-fixable errors need specificity. Retriable errors need a retry action. Blocking errors need honesty and a timeframe if possible. Informational errors need to stay quiet.

### 3. Score

Rate each error message:

- **Clarity** — does it say what happened? (1 = "Something went wrong", 5 = specific diagnosis)
- **Recovery** — does it say what to do? (1 = dead end, 5 = clear next step)
- **Voice** — does it sound like the product? (1 = system-speak, 5 = fully in voice)
- **Blame** — does it blame the user? (1 = accusatory, 5 = neutral or takes responsibility)

Anything below 3 on any axis is a candidate.

### 4. Present candidates

For each candidate:

- **Trigger** — what the user was doing when this appeared
- **Current** — exact current copy
- **Category** — user-fixable, retriable, blocking, or informational
- **Problems** — which axes scored low and why
- **Proposed** — rewritten copy with recovery action

Ask the user which candidates to finalize.

### 5. Finalize

For each selected candidate, write the replacement copy and implement it. For each:

- **Message** — what the user reads (in voice, specific, not apologetic unless the product's voice is apologetic)
- **Recovery action** — button, link, or inline guidance (retry, edit field, go back, contact support)
- **Fallback** — if the specific error can't be determined, what generic-but-still-helpful message to show

## Rules

- Never say "Something went wrong" as the entire message. If you truly don't know what happened, say so honestly: "We couldn't complete that request. Try again, and if it keeps happening, [contact support / here's what to check]."
- Never blame the user for system failures. "Invalid request" when the API is down is a lie.
- Validation errors must reference the specific field and the specific constraint. "Invalid input" is never acceptable.
- Error messages should not apologize unless the product voice apologizes. "Sorry" is filler in most products.
- If an error message includes a raw error code or technical string, it's a bug, not copy.
- Retriable errors must include a retry action, not just suggest retrying in words.
- Don't hide errors that the user needs to know about. A silent failure that corrupts data is worse than an ugly error that saves it.
