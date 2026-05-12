---
name: daily-standup
description: Scan the codebase, issues, and recent commits to generate a prioritized daily work plan of 36-48 hours of optimizations. Use when starting a work session, when deciding what to work on next, when an agent needs autonomous work items, or when you want a fresh-eyes assessment of what the codebase needs most.
---

# Daily Standup

Scan the current state of a codebase and produce a prioritized daily work plan. This is NOT a status report. It is a proactive work-finder that answers "what should I work on today?" by looking at what exists and surfacing what should be done next.

The plan targets 36-48 hours of raw work because, with an AI agent assisting, that typically compresses to 18-24 hours of actual execution. This means the plan stays relevant for a full work session even if priorities shift or incoming requests interrupt.

## Glossary

- **AFK** — work an agent can complete autonomously without human decisions. Well-defined scope, mechanical changes, test-verifiable.
- **HITL** — work that requires human judgment. Ambiguous requirements, design decisions, stakeholder input.
- **Raw hours** — estimated time without AI assistance. The planning target.
- **Compressed hours** — estimated time with AI agent assistance. Typically 40-60% of raw.
- **Signal** — a finding from scanning that suggests work worth doing. Not all signals become work items.
- **Cross-skill signal** — a finding that another skill in the repo would catch during a full audit, surfaced here as a quick check.

## Process

### 1. Scan

Gather signals from multiple sources in parallel. Reference SCAN-SOURCES.md for the specific commands, interpretation guidance, and how to convert findings into candidate work items.

- **GitHub issues** — open issues, labels, staleness, assignment. Use `gh` CLI.
- **Recent commits** — what changed in the last 1-3 days. Use `git log`.
- **Codebase signals** — TODOs, large files, missing tests, lint warnings, type gaps. Use grep, find, and project tooling.
- **Cross-skill signals** — quick checks from other skills: empty states, error copy, security patterns, dead code.

Run these in parallel. Do not read every file. Use targeted commands to surface the highest-signal findings quickly.

### 2. Triage

Evaluate each signal using the prioritization framework in PRIORITIZATION.md. Not every signal becomes a work item.

- Filter noise: cosmetic issues, aspirational TODOs, issues explicitly parked
- Group related signals: if three signals point at the same file or subsystem, that is one work item, not three
- Score each candidate on impact, effort, and AFK/HITL classification
- Apply the 70/30 rule: 70% of the plan should move the product forward (features, fixes, improvements users notice), 30% can be internal quality

### 3. Plan

Assemble 3-5 work items totaling 36-48 raw hours. Reference PLAN-FORMAT.md for the output structure.

- Classify each item as AFK or HITL
- Estimate raw hours and compressed hours
- Order by priority: impact x effort, with AFK items front-loaded
- Check for carryover from yesterday: `git log --since="1 day ago"` for unfinished work
- Verify the plan is balanced: not all refactoring, not all features, not all tests

### 4. Present

Show the plan using the format in PLAN-FORMAT.md. For each item: what, why, estimated hours, AFK/HITL classification, and specific acceptance criteria.

Ask the user to approve, modify, or reprioritize. The plan is a proposal, not a mandate.

If the user approves, AFK items can begin immediately. HITL items go to the user's queue.

## Rules

- Never fabricate signals. Every work item must trace back to something real in the codebase, issue tracker, or commit history.
- Do not scan exhaustively. The scan step should take minutes, not hours. Use targeted commands, not full-codebase reads.
- The plan is a tool, not a commitment. If the user disagrees with a priority, adjust without argument.
- Front-load AFK items. The agent should be able to start working the moment the plan is approved, while the human reviews HITL items.
- Acknowledge what is healthy. If the codebase is in good shape in some dimension, say so. A plan that only lists problems misrepresents reality.
- Check RECURRING-PATTERNS.md when the same signals appear across multiple standups. Recurring patterns indicate structural issues, not just daily tasks.
