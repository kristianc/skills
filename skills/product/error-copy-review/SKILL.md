---
name: error-copy-review
description: Find every error message, toast, validation string, and failure state in a product and rewrite them in the product's voice with recovery actions. Use when the user wants to audit error copy, fix generic error messages, make validation helpful, or ensure every failure state tells the user what went wrong and what to do next.
---

# Error Copy Review

Find every error message in the product and rewrite it so it explains what happened, in the product's voice, with a path to recovery.

Error messages are the moments when the product's relationship with the user is most fragile. Generic errors ("Something went wrong") erode trust. Accusatory errors ("Invalid input") erode goodwill. Silent errors erode both. The goal is to make every failure state honest, helpful, and in voice.

## What counts as error copy

- **Validation messages** — inline field errors, form-level errors, constraint violations (see VALIDATION-MESSAGES.md)
- **API/network errors** — failed requests, timeouts, 4xx/5xx responses surfaced to the user
- **Toasts and banners** — transient error notifications
- **Error pages** — 404, 500, permission denied, not found
- **Empty error states** — screens that show nothing because a fetch failed
- **Console/log messages shown to users** — stack traces, raw error codes, or technical messages that leak through

## Process

### 1. Inventory

Use the Agent tool with `subagent_type=Explore` to find every error message in the codebase. Search for strings containing "error", "fail", "invalid", "wrong", "sorry", "oops", "try again", "went wrong", "unable to", "could not". Check toast/notification calls, validation schemas, error boundary components, HTTP error handlers, and `catch` blocks that surface messages to the UI.

For each error, record: **location** (file, component, line), **trigger** (what causes it), **current copy** (exact text), and **recovery offered** (retry, go back, contact support, or nothing).

### 2. Classify

Sort every error into one of four categories: **user-fixable**, **retriable**, **blocking**, or **informational**. Each category demands a different posture — specificity, retry action, honesty, or quietness respectively. Full definitions, the right communication posture for each, and edge cases where categories blur: see CLASSIFICATION.md.

### 3. Score

Rate each error on four axes — **clarity**, **recovery**, **voice**, and **blame** — on a 1-5 scale. Anything below 3 on any axis is a candidate. Full rubric with concrete examples at each score level: see SCORING-RUBRIC.md.

### 4. Present candidates

For each candidate:

- **Trigger** — what the user was doing when this appeared
- **Current** — exact current copy
- **Category** — user-fixable, retriable, blocking, or informational
- **Problems** — which axes scored low and why
- **Proposed** — rewritten copy with recovery action

Write proposed copy using the patterns in COPY-PATTERNS.md. For inline validation, follow the discipline in VALIDATION-MESSAGES.md.

Ask the user which candidates to finalize.

### 5. Finalize

For each selected candidate, write the replacement copy and implement it:

- **Message** — what the user reads (in voice, specific, not apologetic unless the product's voice is apologetic)
- **Recovery action** — button, link, or inline guidance (retry, edit field, go back, contact support)
- **Fallback** — if the specific error can't be determined, what generic-but-still-helpful message to show

## Rules

- Never say "Something went wrong" as the entire message. If you truly don't know what happened, say so honestly and offer a next step.
- Never blame the user for system failures.
- Validation errors must reference the specific field and the specific constraint. "Invalid input" is never acceptable.
- Error messages should not apologize unless the product voice apologizes.
- If an error message includes a raw error code or technical string, it's a bug, not copy.
- Retriable errors must include a retry action, not just suggest retrying in words.
- Don't hide errors the user needs to know about. A silent failure that corrupts data is worse than an ugly error that saves it.
