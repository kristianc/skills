# GLOSSARY.md

The vocabulary the skill uses. Positioning has a lot of borrowed terms from brand strategy, marketing, and sales — the point of this glossary is to fix the meaning of the ones the skill relies on and exclude the ones it doesn't.

## Core terms

**Positioning** — the deliberate choice of which axes to win on and which to concede. Positioning is not a message, a tagline, or a brand exercise. It is the strategic decision about where to compete. Everything downstream — copy, sales narrative, feature prioritization — follows from it. A product without explicit positioning still has positioning; it's just accidental.

Common misuse: "Our positioning is that we're the easiest to use." That's a claim, not positioning. Positioning is the full structure: we win on ease of setup, for this customer, against these alternatives, and we concede depth of functionality to do it.

**Differentiation** — how the product is different from alternatives on a specific axis. Differentiation is observable fact; positioning is strategic choice. A product can be differentiated on an axis that doesn't matter to the target customer — that's differentiation without positioning value.

Common misuse: using "differentiation" and "positioning" interchangeably. You can be differentiated without being well-positioned (differentiated on the wrong axis) and theoretically positioned without being differentiated (a positioning choice the product hasn't earned yet).

**Category** — the mental shelf the customer puts the product on. "Project management tool." "Email marketing platform." "Data warehouse." The category determines which products the customer compares yours against. Choosing the wrong category means winning a comparison the customer never runs.

Common misuse: inventing a category ("we're a customer success acceleration platform"). If you have to explain the category, the customer will shelve you in an adjacent one and compare you against products optimized for that shelf. New categories work only when the buyer already feels the gap — they know they need something and nothing on the existing shelves fits.

**Wedge** — the intersection of three things: an axis the target customer cares about, where the product is genuinely strong, and where competitors are structurally constrained. The wedge is the core of the positioning document. Without it, positioning is either dishonest (claiming strengths that aren't real) or fragile (claiming strengths competitors can replicate).

Common misuse: treating any advantage as a wedge. A wedge requires all three components. Being strong on an axis competitors can also be strong on is a feature advantage, not a wedge — it has a shelf life measured in quarters.

**Structural constraint** — an architectural decision, business model choice, or market commitment that durably limits what a competitor can do. "They built on a relational database and can't serve sub-10ms queries at scale" is a structural constraint. "They haven't built the feature yet" is not. "Their pricing model requires per-seat revenue, so they can't offer unlimited seats" is a structural constraint. "They charge a lot" is not.

The test: would fixing this require the competitor to make a decision that undermines their existing business, rebuild a core system, or abandon a market they've committed to? If yes, it's structural. If they could ship it in a quarter with a team of five, it's not.

**Claim** — a specific, verifiable statement about the product's strength on an axis. "Sub-200ms p95 latency on queries under 10M rows" is a claim. "Best-in-class performance" is not. "Setup in under 5 minutes with no engineering support" is a claim. "Easy to use" is not.

The test: could a prospect verify this without taking your word for it? Could they test it in a trial, measure it in a POC, or confirm it in a reference call? If the only way to evaluate the claim is to trust the marketing page, it's not a claim — it's an assertion.

**Proof point** — a testable fact, feature, metric, or customer-verifiable characteristic that supports a claim. Proof points are the evidence layer beneath claims. "Open-source core" is a proof point for a data-ownership claim. "SOC 2 Type II certified" is a proof point for a trust claim. "Average setup time of 4.2 minutes across 200 onboardings" is a proof point for an ease-of-setup claim.

Common misuse: listing features as proof points without connecting them to the claim. "We have 200 integrations" is a proof point for an integration-breadth claim, but not for an ease-of-use claim unless you explain why.

**Competitive frame** — the narrative of how the product relates to each major alternative. Not a feature comparison matrix. A competitive frame says: "Unlike X, which optimizes for Y, we optimize for Z because our target customer needs Z more than Y." The frame positions the competitor's strength as a choice they made for a different customer, not as a universal good.

Common misuse: the "we do everything they do, plus more" frame. If this were true, the competitor would not exist. Every competitor is optimized for someone. Name who.

**Alternative** — anything the target customer would do instead of buying the product. Includes direct competitors, adjacent tools repurposed for the job, manual processes, hiring someone, and doing nothing. The most dangerous alternative to ignore is "do nothing / status quo" — it wins more competitive evaluations than any named product.

**Axis** — a dimension customers evaluate when choosing between alternatives. Price, speed, ease of setup, depth, integrations, support, data ownership, customizability, trust. Full treatment in AXES.md.

## Modifiers

**Durable** — an advantage that persists beyond a single product cycle. Structural constraints create durable advantages. Feature leads do not. Positioning should be built on durable advantages.

**Temporary** — an advantage that competitors can erase with effort. First-to-market, a feature they haven't built yet, a price they haven't matched yet. Temporary advantages are worth exploiting in sales but not worth building positioning around.

**Stated vs. revealed** — the difference between what customers say they evaluate on and what they actually evaluate on. Customers say they evaluate on data ownership; they actually evaluate on whether the setup takes more than an hour. See AXES.md for how to distinguish.

## Terms excluded on purpose

- **"Unique value proposition" / UVP** — usually a sentence-completion exercise ("We are the only X that Y") that produces claims no one can verify. Use wedge, claim, and proof point instead.
- **"Competitive advantage"** — too broad. Specify the axis, whether it's structural or temporary, and what it costs.
- **"Market leader"** — a claim about market share dressed as positioning. It says nothing about why a specific customer should choose the product.
- **"Disruptor"** — a self-applied label that communicates nothing to buyers. Name the structural constraint you exploit.
- **"Best-in-class"** — on which axis, measured how, verified by whom? Replace with a claim.
