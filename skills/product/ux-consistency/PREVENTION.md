# PREVENTION.md

Achieving consistency is a project. Maintaining consistency is a practice. This document covers how to keep things consistent once they're standardized — because every inconsistency in a product was consistent at one point, or was never consistent because no one established the pattern in the first place.

## The core problem

Consistency degrades by default. Every new feature, every new developer, every time pressure is a vector for drift. The natural state of a multi-person product is inconsistency. Consistency requires active maintenance — not heroic effort, but small practices applied regularly.

## Establishing naming conventions

Names drift first because they're the easiest to get wrong. A developer building a new feature picks the word that feels right in the moment. If there's no canonical list, they'll guess — and they'll guess differently than the last developer did.

### What to document

Add a **Naming** section to CONTEXT.md with:

- **Entity names.** Every user-facing concept, its canonical name, and explicitly rejected alternatives. "We call it a 'Workspace,' not a 'Team,' 'Space,' or 'Org.'" Include the rejected names — that's what prevents the next person from choosing them.
- **Action labels.** The canonical verb for each action type. "We say 'Delete,' not 'Remove' or 'Trash.' We say 'Create,' not 'Add' or 'New.'" These are the words that go on buttons, in menus, and in confirmation copy.
- **State language.** How the product refers to states in user-facing copy. "We say 'Loading' not 'Fetching.' We say 'Couldn't load' not 'Failed to fetch.'"

### When to establish vs. when to defer

Establish a convention when you've seen the inconsistency — the audit found that "Delete" and "Remove" are both used for the same action. Don't pre-establish conventions for concepts that don't exist yet. Premature naming conventions create overhead without preventing real drift.

## Component audits in PR review

The cheapest place to catch inconsistency is before it merges. Adding a consistency check to PR review doesn't require tooling — it requires questions:

### Questions for every PR that adds UI

1. **Does this use an existing component, or does it create a new one?** If new: is there an existing component that does something similar? If yes, why not use it?
2. **Do the labels match the established naming?** Check button labels, headings, error messages against CONTEXT.md.
3. **Does this match the established pattern for its action type?** If it's a create flow, does it look like the other create flows? If it's a destructive action, does it use the established confirmation pattern?
4. **Does this handle the same states as analogous screens?** Loading, empty, error, success — if the other list screens have all four, this one should too.

These questions don't need to be formal checklist items. They need to be habits. A team that asks "does this match what we do elsewhere?" on every PR will catch drift before it ships.

## The "second instance" rule

From improve-product-feel: one bespoke treatment is a one-off. Two is a pattern worth naming. This rule is the bridge between building and documenting.

### How to apply it

When you're building the second instance of something, stop and ask:

1. Is the first instance the right model? If yes, match it explicitly. If no, fix the first instance too.
2. Is this pattern worth naming? If it'll be used a third time, yes. Add it to CONTEXT.md now, not when the third instance ships.
3. Does the first instance need updating? Sometimes the second instance is better than the first. In that case, update both and document the pattern.

The rule catches patterns at the moment they're forming — before they can diverge. It's less effort than auditing for divergence after the fact.

## When to invest in a design system vs. documented conventions

Not every product needs a design system. Many need documented conventions and discipline more than they need component infrastructure. The dividing line:

### Documented conventions are enough when

- The team is small enough (1-5 developers) that everyone can hold the conventions in their head after reading CONTEXT.md
- The product is young enough that conventions are still forming — premature systematization creates rigidity before you know what the patterns should be
- The inconsistencies are primarily in naming, copy, and interaction patterns — things that tooling can't enforce anyway
- The team has the discipline to check CONTEXT.md during development and review

### A design system earns its investment when

- The team is large enough (5+ developers) that verbal conventions don't reach everyone
- The same component genuinely appears in ten or more places and visual drift is the primary consistency problem
- The product is mature enough that the patterns are stable — you're standardizing, not still discovering
- The inconsistency is primarily visual (spacing, sizing, colors, component variants) — the kind of thing a component library directly prevents

### The in-between: component conventions without a design system

For teams between "documented conventions" and "full design system," there's a middle ground: name the component patterns in CONTEXT.md without building infrastructure. "Cards use 8px border-radius, 16px padding, and 1px border" is a convention a developer can follow without a Card component that enforces it. It's cheaper to establish and still catches most visual drift.

## How consistency practices scale

### Solo developer

Your main enemy is yourself-from-six-months-ago. Document conventions in CONTEXT.md as you establish them. Run this audit every few months to catch your own drift. The "second instance" rule is your primary tool — every time you build the second version of something, name the pattern.

### Small team (2-5)

Everyone can read CONTEXT.md and hold it in memory. The main risk is new team members who don't know the conventions yet. Onboarding should include reading CONTEXT.md. PR review should include the consistency questions above. Run this audit before any major launch — it's cheap insurance.

### Growing team (5-15)

CONTEXT.md alone won't scale. Supplement with:
- Linting rules for the conventions that can be automated (naming patterns, import patterns, component usage)
- A component inventory that lives alongside the codebase (Storybook, Catalogue, or even a markdown file with screenshots)
- A designated consistency owner (not a full-time role — a rotating responsibility during PR review)
- Quarterly consistency audits using this skill

### Large team (15+)

At this scale, a design system is worth the investment if you don't have one. Documented conventions still matter for the dimensions a design system can't enforce (naming, interaction patterns, copy, flows). Add:
- Design tokens enforced at the build level
- Component libraries with variants, not freestyle props
- Copy guidelines as a linting step (capitalize this way, use these words)
- Flow templates for common task types (create, edit, delete, list)

## Preventing the next audit from finding the same problems

The highest-value output of a consistency audit is not the fixes — it's the patterns documented in CONTEXT.md that prevent recurrence. After every audit:

1. **Check that every fixed inconsistency has a corresponding pattern in CONTEXT.md.** If you standardized a button label but didn't document the convention, the drift will return.
2. **Check that every pattern has enough specificity to apply.** "Be consistent" is not a pattern. "Primary actions use 'Save' for persisting changes" is.
3. **Check that intentional variations are documented with their rationale.** If a future developer encounters the variation and can't find the reason, they'll either "fix" it (breaking the intention) or leave it (starting accidental drift elsewhere).
4. **Set a reminder to re-audit.** The cadence depends on team velocity: monthly for fast-shipping teams, quarterly for steady-pace teams, before every major launch for everyone.
