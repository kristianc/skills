---
name: handoff
description: Turn conversation context into a spec and independently-grabbable issues. Use when the user wants to turn decisions into work items, create issues from a conversation, break down a plan, publish a spec, or hand off work to other agents or teammates.
---

# Handoff

Synthesize the decisions in a conversation into a spec, break it into vertical-slice issues, and publish them to the issue tracker. The output is work that humans or agents can grab independently.

This skill does NOT interview the user. Everything it needs is already in the conversation context — decisions made, findings surfaced, actions identified. If the conversation didn't produce enough decisions, the right move is to go back and finish the conversation, not to generate placeholder decisions here.

## Glossary

Use these terms precisely. Full definitions in GLOSSARY.md.

- **Spec** — a synthesis artifact that captures what was decided, not what was discussed. Not a PRD, not a brief, not a ticket.
- **Vertical slice** — a thin end-to-end cut through all layers that delivers visible, demoable value. Not a horizontal layer.
- **HITL** — human-in-the-loop. An issue that requires human judgment to complete.
- **AFK** — an issue an agent can complete autonomously, without human decisions.
- **Acceptance criteria** — independently testable conditions that define done.

## Process

### 1. Gather

Read back through the entire conversation. Extract every:

- **Decision** — a choice that was made, including what was rejected and why
- **Finding** — something discovered that affects the work (a vulnerability, a friction point, a constraint)
- **Action item** — something that needs to happen, stated or implied

Do not interview. Do not ask clarifying questions unless the conversation is genuinely ambiguous on a point that changes the shape of the work. Synthesize what is already there.

### 2. Draft the spec

Structure the gathered material into a spec using the format in SPEC-FORMAT.md. The spec is a synthesis artifact — it captures what was decided, not the discussion that led there.

Present the spec to the user for review. Ask: "Does this capture the decisions accurately? Anything missing, wrong, or out of scope?"

Iterate until the user confirms the spec reflects reality.

### 3. Slice into issues

Break the spec into vertical-slice issues. Each issue is a thin cut through ALL layers — schema, API, UI, tests — not a horizontal layer. See SLICING.md for the methodology.

Classify each issue as HITL or AFK. Prefer AFK — the more issues agents can grab independently, the faster the work moves. Reserve HITL for issues that genuinely require human judgment (copy decisions, design choices, business logic ambiguities).

### 4. Quiz the user

Present the issue breakdown as a numbered list with titles, types (HITL/AFK), and one-line summaries. Let the user:

- Merge issues that are too thin
- Split issues that are too thick
- Reorder priorities
- Reclassify HITL/AFK
- Adjust scope

Iterate until the user approves the breakdown.

### 5. Publish

Create issues in dependency order — blockers first, so downstream issues can reference real IDs. Use the template in ISSUE-FORMAT.md.

Link each issue to the spec. Link each issue to its blockers using real issue IDs, not placeholders. Add labels for HITL/AFK classification.

Report back with the list of created issues and their IDs.

## Rules

- Never invent decisions that weren't made in the conversation. If the conversation didn't decide something, it goes in "Open questions," not in "Decisions."
- No file paths or code snippets in issues. They go stale the moment someone commits. Decisions and acceptance criteria only.
- Every issue must be independently grabbable — a developer or agent should be able to pick it up without reading all the other issues first (though they may be blocked by other issues).
- Publish in dependency order. Never reference an issue ID that doesn't exist yet.
- The spec is the source of truth. Issues are slices of the spec, not independent documents. If an issue contradicts the spec, the spec wins.
