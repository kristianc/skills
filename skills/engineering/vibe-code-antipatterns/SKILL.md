---
name: vibe-code-antipatterns
description: Audit a codebase for structural problems common in AI-generated code — god components, missing error handling, copy-paste duplication, inconsistent patterns, and happy-path-only logic. Use when inheriting a vibe-coded project, before refactoring AI-generated code, when a codebase "works but feels fragile," or when onboarding someone to a project that grew without architectural intention.
---

# Vibe Code Antipatterns

Systematically audit a codebase for the structural problems that AI-generated and "vibe coded" projects accumulate. The goal is not to find bugs — the code works. The goal is to find the reasons it will stop working the moment someone tries to change it.

Vibe-coded projects fail not because the code is wrong but because the code is unmaintainable. It works today but cannot be extended, debugged, or handed off. This skill identifies that structural debt and provides incremental paths to fix it.

## The core problem

AI code generators optimize for "working code in one shot." This produces specific failure patterns: god components, no separation of concerns, copy-paste duplication, happy-path-only code, hardcoded everything, TODO-as-architecture, inconsistent patterns, and over-abstraction in the wrong places. The same audit framework applies to any codebase that grew without architectural intention — AI just produces these smells faster and more consistently than humans.

## Process

### 1. Scope

Establish what is being audited: the full application, a specific feature, or recent AI-generated code. Ask the user if not obvious.

Identify the framework and stack so the patterns are relevant. A React app has different smells than a Rails app — god components vs. fat controllers, hooks vs. concerns.

### 2. Scan for smells

Walk the codebase using the taxonomy in SMELL-TAXONOMY.md and the detection shortcuts in DETECTION-PATTERNS.md. Do not read every file. Start with:

- The largest files (by line count) — god components live here
- The most-imported modules — shared dependencies reveal architecture (or lack of it)
- The entry points — route definitions, main components, index files

Use DETECTION-PATTERNS.md for the specific commands and grep patterns that surface each smell category quickly.

### 3. Score severity

For each smell found, score using SCORING-RUBRIC.md on three axes: **fragility** (will it break?), **contagion** (does it force other code to be worse?), and **fix cost** (how expensive is the refactor?).

The triage question: "What happens if a junior dev needs to modify this code tomorrow?"

Distinguish between "annoying but harmless" and "will break when you try to change it." Not everything needs fixing. Some smells are cosmetic. Others are load-bearing structural problems.

### 4. Present findings

Organize by severity (critical first, then high, medium, low). For each finding:

- **What** — the smell, named from the taxonomy
- **Where** — file, function, line range
- **Why it matters** — the specific future failure mode this enables
- **Fix direction** — one sentence on what the fix looks like (full details in REFACTORING-PLAYBOOK.md)

Ask the user which findings to address.

### 5. Refactor

Apply the patterns from REFACTORING-PLAYBOOK.md. Make incremental, testable changes. Do not rewrite everything at once.

For each refactoring move:
- Verify the existing behavior before changing anything
- Make the smallest change that addresses the smell
- Verify behavior is preserved after the change
- Move to the next smell

## Rules

- Every finding must include a fix direction. A smell report without remediation is a complaint, not an audit.
- Do not conflate "code I would write differently" with "code that is structurally broken." Personal style preferences are not findings.
- The golden rule: never refactor something you do not have a test for (or cannot manually verify). The worst outcome is a cleaner codebase that is also broken.
- Acknowledge what works. AI-generated code often has good naming, decent component boundaries at the leaf level, and solid library choices. Say so.
- Be specific. "This code is messy" is not a finding. "This 340-line component handles routing, API calls, state management, and rendering for three different views" is.
