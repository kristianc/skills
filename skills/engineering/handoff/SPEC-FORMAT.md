# SPEC-FORMAT.md

The template for the synthesis artifact this skill produces. A spec captures what was decided, not what was discussed. Someone who wasn't in the conversation should be able to read the spec and understand what to build and why, without asking follow-up questions.

## Template

```markdown
# [Title]

## Problem

[What's wrong, from the user's perspective. Not "the API doesn't handle X" but "users lose their work when Y happens." The problem statement should make sense to someone who has never seen the codebase.]

## Solution

[What changes, stated in user terms. Not "add a new endpoint" but "users can now do X, which means Y." The solution should be understandable without knowing the implementation. One to three paragraphs.]

## Decisions

[Every decision crystallized in the conversation. This is the most important section.]

### [Decision 1 title]

**Decided:** [what was chosen]
**Alternatives considered:** [what was rejected]
**Rationale:** [why this choice, not just "it's better" — what specific trade-off was made]

### [Decision 2 title]

...

## Scope boundaries

**In scope:**
- [what this spec covers, stated concretely]

**Out of scope:**
- [what this spec explicitly does not cover] — [why]

## Open questions

- [anything unresolved that the implementer needs to decide or the team needs to discuss]
```

## Section guidance

### Problem

The problem section answers: why does this work need to happen?

**Good:** "When a security review finds 15 vulnerabilities, there's no structured way to turn the findings into trackable work items. The reviewer copies findings into issues one at a time, losing context about priorities and dependencies between fixes."

**Bad:** "We need a way to create issues from findings." (Too vague — doesn't explain the current pain or who experiences it.)

**Bad:** "The SecurityFindingsService doesn't integrate with the IssueTracker module." (Implementation perspective, not user perspective.)

Write the problem from the perspective of the person who will benefit from the solution, not from the perspective of the system. If the conversation was a polish review, the problem is the user friction that was found. If it was a security audit, the problem is the exposure. If it was a pricing analysis, the problem is the revenue being left on the table.

### Solution

The solution section answers: what's different when this work is done?

**Good:** "After a security review, the reviewer can generate a prioritized set of issues in the tracker, each linked to the spec, with dependencies wired so that blockers are addressed first. Issues are classified as agent-executable or requiring human judgment, so the team can parallelize remediation."

**Bad:** "Add a handoff flow that creates issues." (Doesn't say what changes for the user or why it matters.)

State the solution in terms of what the user can now do, not in terms of what the system now has. Avoid implementation details — the solution should survive a complete rewrite of the architecture.

### Decisions

The decisions section is the heart of the spec. Every decision made during the conversation belongs here, including:

- **Approaches chosen** — with enough rationale that someone who wasn't in the conversation understands why
- **Approaches rejected** — what was considered and why it was dropped. Rejected alternatives are as important as chosen ones; they prevent the implementer from re-discovering and re-litigating paths already explored
- **Constraints acknowledged** — technical limits, business rules, timeline pressures, resource constraints that shaped the decisions
- **Patterns adopted** — if the conversation established a pattern ("all destructive actions get undo toasts"), capture it here
- **Trade-offs accepted** — every non-obvious trade-off, stated explicitly. "We chose speed over completeness because the launch date is fixed" is a trade-off. "We chose the good option" is not.

**Good decision entry:**

```
### Issue classification: HITL vs. AFK

**Decided:** Default to AFK. Only classify as HITL when the issue
genuinely requires human judgment — copy decisions, design review,
business logic ambiguity.

**Alternatives considered:** Default to HITL (safer but slower — every
issue becomes a bottleneck). Hybrid with auto-detection (adds complexity
and the detection heuristic would need constant tuning).

**Rationale:** The goal is to maximize parallel execution. An issue
incorrectly classified as AFK will fail and get reclassified. An issue
incorrectly classified as HITL sits in a queue. The cost of false AFK
is lower than the cost of false HITL.
```

**Bad decision entry:**

```
### Classification

**Decided:** Use HITL and AFK labels.
```

(No alternatives, no rationale, no trade-off. The implementer doesn't know why, and the next person to revisit will re-litigate from scratch.)

### Scope boundaries

Scope is a two-sided boundary. In-scope items are commitments; out-of-scope items are explicit exclusions with reasons.

**Good out-of-scope entry:** "Automated test generation for each issue — the acceptance criteria are enough for a human or agent to write tests; generating the tests themselves is a separate concern and adds complexity to the handoff flow."

**Bad out-of-scope entry:** "Nice to have: auto-generate tests." (Not a scope boundary — it's a wishlist item sneaking back in.)

The reason matters. "Out of scope because the conversation decided against it" is a decision. "Out of scope because it's a separate piece of work" is a scope cut. "Out of scope because we don't know how yet" is an open question mislabeled as a scope boundary — move it to open questions.

### Open questions

Open questions are unresolved items that the implementer or the team needs to decide. They are not parking lots for ideas or wishlists for future work.

**Good open question:** "Should the spec be stored as a GitHub issue (visible in the tracker, but limited formatting) or as a markdown file in the repo (full formatting, but not in the tracker)? The conversation discussed both but didn't land."

**Bad open question:** "What other features could we add?" (Not a question that blocks implementation.)

Each open question should state: what needs to be decided, who can decide it, and what happens if it's not decided (can the work proceed with a default, or is it blocked?).

## Source-specific guidance

The spec format is the same regardless of what kind of conversation produced the decisions. But the content emphasis shifts.

**From a polish review (improve-product-feel):** The problem section emphasizes user friction. Decisions focus on touchpoint treatments chosen. Scope boundaries clarify which touchpoints are addressed now vs. later.

**From a security audit (security-review):** The problem section describes the exposure. Decisions include the remediation approach for each finding and the priority order. Scope may exclude low-severity findings explicitly.

**From a production readiness review (production-ready):** The problem section describes the operational gaps. Decisions focus on which dimensions to address and to what level. Scope boundaries distinguish "launch-blocking" from "post-launch."

**From a pricing analysis (pricing-research):** The problem section describes the pricing pain (leaving revenue on the table, misaligned tiers, competitive pressure). Decisions capture pricing structure choices. Open questions often include "need to validate with customers."

**From a positioning exercise (competitive-positioning):** The problem section describes the market gap or positioning weakness. Decisions include axes chosen, claims to make, and claims to avoid. Scope excludes messaging (that's a separate brief).

**From an ICP definition (ideal-customer-profile):** The problem section describes who the product is trying to serve. Decisions capture qualifying and disqualifying criteria. Open questions often include segments that need more data.

In every case, the spec is a synthesis artifact. It captures what was decided, not what was discussed. If the conversation was three hours of exploration followed by five minutes of crystallization, the spec reflects the five minutes.
