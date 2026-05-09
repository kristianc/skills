# GLOSSARY.md

The vocabulary the skill uses. Precise language prevents the most common handoff failures: work that doesn't match what was decided, issues that can't be picked up independently, and specs that are really just meeting notes.

## Core terms

**Spec** — a synthesis artifact that captures what was decided. A spec is not a transcript of the conversation, not a list of things discussed, and not a brainstorm. It contains: the problem, the solution, the decisions made (with rationale), the scope boundaries, and any open questions. A spec is complete when someone who wasn't in the conversation can understand what to build and why without asking follow-up questions.

**PRD (Product Requirements Document)** — a broader document that covers the full lifecycle of a feature: user research, market context, success metrics, rollout plan, stakeholder sign-offs. A spec is a subset of a PRD. This skill produces specs, not PRDs. If you need a PRD, write it separately and reference the spec.

**Brief** — a short document that frames a problem and proposes an approach, used to get alignment before work begins. A brief precedes a spec. The launch-brief skill produces briefs; this skill consumes the decisions from a brief and produces a spec and issues.

**Ticket** — a unit of work in a tracker (GitHub issue, Linear issue, Jira ticket). A ticket is a slice of a spec. This skill produces tickets from specs. Don't call the spec a ticket, and don't call the tickets a spec.

**Vertical slice** — a thin end-to-end cut through ALL layers of the system that delivers visible, demoable value. A vertical slice for "user can reset their password" touches the UI (reset form), the API (reset endpoint), the database (token storage), email (reset link), and tests. It is independently deployable and independently verifiable. The user can see the result.

**Horizontal slice** — a cut through a single layer of the system. "Build the password reset API endpoint" is a horizontal slice. It's not independently demoable (no UI), not independently verifiable by a user (requires calling the API directly), and creates integration risk (what if the UI team built something different?). Horizontal slices are the default way most teams break down work. They are almost always wrong.

**Tracer bullet** — the thinnest possible vertical slice that proves the architecture works end-to-end. The first issue in a breakdown is often a tracer bullet: the simplest case, wired all the way through, with minimal error handling. If the tracer bullet works, the rest is incremental. If it doesn't, you learn early.

**HITL (Human-In-The-Loop)** — an issue that requires human judgment to complete. Typical reasons: the issue involves a copy decision the conversation didn't settle, a design choice that needs visual review, a business logic ambiguity that only a domain expert can resolve, or a trade-off where the right answer depends on context the agent doesn't have.

**AFK (Agent-Autonomous)** — an issue an agent can complete without human decisions. The acceptance criteria are specific enough, the implementation path is clear enough, and the decisions are all made. AFK doesn't mean trivial — an AFK issue can be complex, as long as the complexity is implementation, not judgment. Prefer AFK. Every issue that could be AFK but is classified as HITL is a bottleneck.

**Acceptance criteria** — independently testable conditions that define "done" for an issue. Each criterion is a single statement that can be verified without judgment calls: "the reset email arrives within 30 seconds" is testable; "the reset flow feels good" is not. Acceptance criteria are the contract between the spec and the implementation.

**Dependency** — an issue that must be completed before another issue can start. Dependency is not "it would be nice to have this first" — it's "this issue literally cannot be built without the output of that issue." Minimize dependencies. Every dependency is a serialization point that slows the work down.

**Blocker** — a dependency that is not yet resolved. An issue with an unresolved blocker cannot be started. The publish step creates issues in dependency order specifically so that blockers have real IDs that downstream issues can reference.

**Scope** — the boundary of what the spec covers. Scope is defined positively (what's in) and negatively (what's explicitly out and why). The "why" matters: "out of scope because the conversation decided against it" is different from "out of scope because we'll do it later" is different from "out of scope because it's someone else's problem."

**Decision** — a choice made during the conversation, including the alternatives considered and the rationale for the choice. Decisions are the most important content in a spec. An implementation can survive without knowing the full discussion, but it cannot survive without knowing what was decided and why. Decisions include: approach chosen, approaches rejected, constraints acknowledged, patterns adopted, trade-offs accepted.

**Finding** — something discovered during the conversation that affects the work. In a security review, findings are vulnerabilities. In a polish review, findings are friction points. In a pricing analysis, findings are pricing sensitivities. Findings become inputs to the spec; they are not the spec themselves.

**Action item** — something that needs to happen, identified during the conversation. Action items become issues. Not all action items are equal — some are decisions that need to be made (HITL), some are implementations that can be executed (AFK), and some are investigations that need to happen before the spec is complete (open questions).

## Common misuses

**Treating a user story as a spec.** "As a user, I want to reset my password so that I can regain access to my account" is a problem statement, not a spec. It doesn't capture decisions (email or SMS? Token expiry? Rate limiting?), scope boundaries (do we also handle locked accounts?), or acceptance criteria. User stories are fine as inputs; they are not outputs of this skill.

**Treating a horizontal layer as a vertical slice.** "Build the API" / "Build the UI" / "Write the tests" is three horizontal slices, not three vertical slices. The API team builds something, the UI team builds something else, and the integration breaks. Vertical slices avoid this by forcing end-to-end thinking in every issue.

**Treating scope as a wishlist.** Scope is not "everything we'd eventually like to do." It's "what this spec covers." Out-of-scope items belong in a separate section with reasons, not as "nice to haves" or "stretch goals" inside the spec.

**Treating acceptance criteria as implementation steps.** "Add a POST /reset endpoint" is an implementation step. "A user can request a password reset by submitting their email address" is an acceptance criterion. The difference: acceptance criteria describe what's true when the work is done, not how to get there. Implementation steps go stale and constrain the implementer unnecessarily.

**Confusing blocked with unstarted.** An issue is blocked when it cannot proceed because a dependency is unresolved. An issue is unstarted when no one has picked it up yet. Calling unstarted issues "blocked" hides real bottlenecks.
