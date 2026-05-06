# MESSAGE-TESTING.md

Once the audience is locked, the message is the next load-bearing decision. This file covers how to build the headline claim, choose a narrative arc, select proof points, and stress-test the result until it is sharp enough to launch.

The goal is not clever copy. The goal is a claim so specific that the target audience stops scrolling and says "that is for me."

## The headline claim

The headline claim is one sentence stating what changed and why the audience should care. It is not the final headline — copywriters will polish it later. It is the strategic claim that all copy will derive from.

Structure: **what changed** + **why they should care**.

- "What changed" is the product fact. Something is new, faster, cheaper, possible for the first time.
- "Why they should care" is the audience connection. It maps the product fact to something the audience is actively dealing with.

Both halves are required. A claim that is all product fact ("We rebuilt our query engine") has no audience connection. A claim that is all audience connection ("Faster insights for your team") has no product fact. The headline claim needs both.

**Examples:**

- Weak: "New query engine." (Product fact only, no audience connection.)
- Weak: "Faster analytics for everyone." (Audience connection only, no product fact, and "everyone" is not an audience.)
- Strong: "Query results in under 2 seconds on datasets up to 50M rows — so you stop waiting and start exploring." (Product fact: sub-2s on 50M rows. Audience connection: they are currently waiting, and the wait breaks their flow.)
- Strong: "One-click SOC 2 compliance reports, formatted for your auditor." (Product fact: automated, formatted reports. Audience connection: they are currently spending hours on this manually.)

## How to know if the claim is specific enough

A claim is specific enough when it is **specific enough to be wrong**.

"Faster analytics" cannot be wrong — it commits to nothing. It is unfalsifiable, which means it is also uninteresting. Nobody stops scrolling for unfalsifiable claims.

"Query results in under 2 seconds on datasets up to 50M rows" can be wrong. Someone could test it and find it takes 5 seconds on 30M rows. That risk is exactly what makes it credible — the specificity signals that the team actually measured it.

Apply this test to every claim: could a skeptical user disprove this? If not, it is not specific enough.

A related test: **is this claim specific enough to be boring to someone outside the target audience?** If the claim is interesting to everyone, it is probably too vague. A claim about SOC 2 compliance reports should bore a frontend developer. A claim about sub-2s queries on 50M rows should bore a marketing manager. That boredom is a signal that you are talking to someone specific.

## The three narrative arcs

Every launch tells a story. The story has a structure. Pick one.

### Problem to solution

**Structure:** You have been dealing with X. Now you can Y.

**When it fits:** The audience has a known, named problem. They talk about it. They search for solutions. The problem is painful enough that naming it creates recognition — "yes, that is exactly what I deal with."

**Example:** "Data pipeline debugging is a nightmare — errors surface hours after the pipeline runs, in logs nobody reads. Pipeline Monitor catches errors in real time and tells you which step failed and why."

**Strength:** Creates immediate recognition. The audience self-selects — if they have the problem, they lean in. If they do not, they move on, which is correct.

**Risk:** If the problem is not widely felt, naming it creates confusion rather than recognition. Do not use this arc for problems the audience does not already know they have.

### Before to after

**Structure:** This used to take X. Now it takes Y.

**When it fits:** The audience already does the thing the product improves. They know what it costs them in time, effort, or money. The improvement is quantifiable and dramatic enough to be interesting.

**Example:** "Compliance reports used to take 20 hours per quarter. Now they take one click."

**Strength:** Concrete. The audience can do the math in their head. "20 hours to one click" is not an abstraction — it is a specific relief they can feel.

**Risk:** If the before state is not painful enough, the comparison falls flat. "This used to take 5 minutes, now it takes 3" is technically a before-to-after arc, but the delta does not justify a launch. Use this arc only when the gap is dramatic.

### Limitation to capability

**Structure:** Until now, you could not X. Now you can.

**When it fits:** The launch enables something genuinely new — not an improvement to something existing, but a capability that did not exist before. The audience has either worked around the limitation or accepted it as a constraint of the category.

**Example:** "Until now, real-time collaboration on data models meant everyone staring at the same screen. Now you can edit simultaneously from anywhere, with live conflict resolution."

**Strength:** Creates excitement. New capabilities generate more enthusiasm than improvements because they expand what is possible, not just what is efficient.

**Risk:** If the audience does not recognise the limitation, "now you can" has no impact. They shrug — "I did not know I could not." Use this arc only when the limitation is something the audience has consciously bumped into.

### Choosing the arc

The arc follows from the audience grilling:

- If the audience named a problem in step 2 ("what do they care about right now?"), use **problem to solution**.
- If the audience is already doing the thing and the improvement is quantifiable, use **before to after**.
- If the launch enables something the audience could not do at all, use **limitation to capability**.

Do not mix arcs in a single launch. A message that starts with "you have been dealing with X" (problem-to-solution) and ends with "now you can do something entirely new" (limitation-to-capability) confuses the reader about whether this is an improvement or a new thing. Pick one and commit.

## Proof points

The claim without proof is an assertion. Proof points are the 2-3 specific facts that make the claim credible.

### What counts as a proof point

- **A number.** "Sub-2-second queries on 50M rows." "10x higher rate limits." "One click instead of 20 hours." Numbers are proof because they can be checked.
- **A case study.** "Company X reduced compliance time by 80%." Specific, named, verifiable.
- **A concrete capability.** "Edit simultaneously with live conflict resolution." This is proof because it describes a mechanism — it is not an adjective, it is a thing the product does.

### What does not count

- **Adjectives.** "Best-in-class," "industry-leading," "powerful," "seamless." These are assertions pretending to be proof.
- **Vague comparisons.** "Faster than before." How much faster? Compared to what?
- **Internal metrics.** "Built on our new architecture." The audience does not care about your architecture. They care about what it means for them.
- **Future capabilities.** "Will support X by Q3." Proof points must be true at launch. Promises are not proof.

### Selecting proof points

Choose proof points that serve the primary audience and the chosen narrative arc:

- For **problem to solution**, the proof shows the problem is actually solved — not just addressed, but solved. "Catches errors in real time" is better than "improves error handling."
- For **before to after**, the proof quantifies the delta. The bigger and more specific the number, the better.
- For **limitation to capability**, the proof demonstrates the capability is real. A demo, a screenshot, a concrete description of the mechanism.

Two to three proof points is the right number. One is not enough to be credible. Four or more dilutes the message — the reader cannot remember all of them, and listing many proof points signals insecurity about each individual one.

## Testing the message

Four tests, run in order. Each catches a different failure mode.

### The bar test

Describe the launch to a stranger at a bar. Not a colleague, not someone in the industry — a stranger. If they cannot understand why someone would care, the message is too insular.

This test catches jargon, assumed context, and inside-baseball framing. "We rebuilt our query engine using columnar storage" fails the bar test. "Your reports now load in 2 seconds instead of 30" passes.

The bar test is not about dumbing down the message. It is about ensuring the core claim is intelligible outside the product team's bubble. The final copy can be as technical as the audience requires — but the strategic claim should survive the bar test.

### The competitor test

Could a competitor make the same claim? If yes, the claim is not differentiated.

"Faster analytics" — every analytics company claims this. Not differentiated.

"Sub-2-second queries on datasets up to 50M rows" — this is a specific, measurable claim. A competitor could make it too, but only if they actually achieve it. The specificity forces differentiation because most competitors cannot make the same specific claim truthfully.

The competitor test catches lazy messaging. If the claim could appear on any competitor's website without modification, it is not a claim about your product — it is a claim about the category.

### The "so what" test

Read the headline claim aloud. Then ask "so what?" Answer. Then ask "so what?" again. Answer. Then ask "so what?" a third time.

If you run out of answers before the third "so what," the claim does not go deep enough.

**Example:**

- Claim: "We launched real-time collaboration on data models."
- So what? "Teams can edit simultaneously instead of taking turns."
- So what? "Data modeling bottlenecks disappear — the team moves at the speed of the fastest person, not the slowest."
- So what? "Projects that used to take two weeks of back-and-forth now converge in a single working session."

The third answer is often the real message. "Projects converge in a single session" is more compelling than "we launched real-time collaboration." The "so what" test drills down to the audience impact that the product framing obscures.

If the user cannot get past the first "so what," the launch may not matter enough to the audience to justify the launch. This is a real finding — some features are genuinely not launch-worthy, and the skill should say so.

### The honest test

Is every proof point currently true? Not "will be true by launch" — true right now, or true with high confidence by the launch date.

This test catches over-promising. A launch with proof points that require caveats ("works best when...," "currently limited to...") is either a launch with caveats built into the copy, or a launch that should wait. The honest test forces this decision before creative work begins.

If a proof point is almost true — "it is 1.8 seconds, not sub-2 seconds, but it will be by launch" — include it, but note the dependency in the brief. The brief is the single source of truth for what can be claimed.

## Sharpening a claim that is too vague

When a claim fails the tests above, it usually fails for one of these reasons. Each has a specific remedy.

**The claim is about the product, not the audience.** Rewrite to start from the audience's situation. "We rebuilt our engine" becomes "your queries now return in 2 seconds."

**The claim is a category claim, not a product claim.** Add specificity — a number, a mechanism, a constraint. "Better security" becomes "SOC 2 compliance reports in one click."

**The claim uses adjectives instead of evidence.** Replace every adjective with a fact. "Powerful new API" becomes "API with 10x higher rate limits and batch endpoints."

**The claim tries to serve two audiences.** Split into two claims, one per audience. Choose the primary claim for the brief; the secondary can appear in channel-specific copy.

**The claim hedges.** Remove the hedge and see if the claim is still true. "Helps teams collaborate more effectively" becomes "real-time simultaneous editing on data models." If removing the hedge makes the claim false, the claim is not ready.
