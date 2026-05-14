# CLASSIFICATION.md

Not all inconsistency is a bug. Some of it is a justified design decision. The distinction matters because the resolution is completely different: accidental variation gets standardized, intentional variation gets documented, drift gets updated, and conflicts get decided.

Classifying correctly prevents two failure modes: standardizing something that was intentionally different (breaking a good decision), and documenting something that was accidentally different (legitimizing a mistake).

## 1. Intentional variation

### What it is

Different context genuinely justifies different treatment. The inconsistency is deliberate and defensible. Someone made a conscious choice to handle this case differently, and the reason holds up when examined.

### Examples

- A confirmation modal for deleting a payment method (irreversible, financial) but an undo-toast for deleting a draft (reversible, low-stakes). The action is the same (delete) but the stakes justify the difference.
- A compact form layout in a sidebar but a spacious layout on a full page. The component is the same but the context is different.
- Formal copy in legal and billing sections but conversational copy in the main product. The voice shifts intentionally at a clear boundary.

### How to tell

Ask: "If you explained this difference to someone who didn't build it, would they agree it makes sense?" If the answer is yes — if the reason is about the user's situation, not about who happened to build it — it's intentional.

Another test: "Would making these consistent actually make the product worse?" If forcing the same treatment would harm one context (a casual undo-toast for payment deletion, a heavyweight modal for deleting a draft), the variation is justified.

### How to resolve

Don't standardize. Document. Add the exception to CONTEXT.md so the pattern reads: "Destructive actions get undo-toasts — except for irreversible financial actions, which get confirmation modals." The documentation prevents the next developer from "fixing" the intentional variation.

If there's no documentation and you believe the variation is intentional, ask the user to confirm before either standardizing or documenting. Don't guess.

---

## 2. Accidental variation

### What it is

Different developers made different choices without knowing about each other's work. No one decided to be inconsistent — the inconsistency happened because the right hand didn't know what the left hand was doing. This is the most common type in products built by multiple people or over extended timelines.

### Examples

- One developer built the user settings page with "Save changes" and another built the project settings page with "Apply." Neither knew about the other's label.
- Delete confirmation uses a modal in the contacts section (built in Q1 by developer A) and an undo-toast in the tasks section (built in Q3 by developer B). No decision was made — it just happened.
- Loading states show a spinner in the dashboard (the first screen built) and a skeleton in the inbox (built later when the team had learned about skeletons). The dashboard was never updated.
- Error messages in the billing section say "Unable to process your request" while the main app says "That didn't work." Different writing styles, no style guide.

### How to tell

Ask: "If you asked 'why is this different?' would the answer be 'I didn't know about the other one'?" If the developer who built instance B would have matched instance A had they known about it, it's accidental.

Another test: there's no product reason for the difference. The entities are similar weight, the contexts are similar, the user expectations are similar. The only explanation is that two people worked independently.

### How to resolve

Standardize. Pick the canonical version (usually the better one, or the more common one if quality is similar), apply it everywhere, and document the pattern in CONTEXT.md. If there's no clear winner, let the user decide.

When standardizing, check for downstream effects. A button label change is isolated; a confirmation pattern change might require adding undo infrastructure or changing how deletions work.

---

## 3. Evolutionary drift

### What it is

Something was consistent, then one or more instances were updated and the others weren't. The product evolved unevenly. This is the "we redesigned the settings page but forgot about the modal that also shows settings" pattern.

### Examples

- The product migrated from spinners to skeletons for loading states, but three screens still show spinners because they weren't included in the migration.
- The team decided "Save" is the canonical button label, updated most forms, but missed two.
- A component library upgrade changed the default card border-radius from 8px to 12px, but some cards use a hardcoded `border-radius: 8px` that overrides the default.
- The product's voice shifted from formal to conversational over time, but error messages written two years ago still say "An error has occurred. Please contact support."

### How to tell

There's a clear before/after. One version is newer, the other is older. If you check git history, you can usually find the point where the change was made and see which instances were updated. The old instances weren't intentionally kept — they were overlooked.

Another signal: the inconsistency follows a time gradient. Newer screens have one pattern, older screens have another. The inconsistency maps to build dates, not to context.

### How to resolve

Complete the migration. The canonical version is the newer one (since the old one was explicitly replaced). Apply it to the remaining instances. This is usually the easiest fix because the pattern is already established — it just wasn't applied everywhere.

Track what was missed and why. If certain screens get repeatedly missed during updates, they may need to be included in a checklist or automated test. See PREVENTION.md.

---

## 4. Precedent conflict

### What it is

Two competing patterns, both intentional, both used broadly. Neither is "wrong" — there's just no canonical choice. This happens when two teams independently established conventions, or when a product evolved two legitimate approaches and no one reconciled them.

### Examples

- Half the product uses modals for create actions and half uses full-page forms. Both patterns have been deliberately applied to multiple entities. Neither is a mistake — there are two schools of thought on the team.
- Some sections use sentence case for headings ("Your projects") and others use title case ("Your Projects"). Both are applied consistently within their sections, but the sections disagree.
- The API uses `snake_case` for some endpoints and `camelCase` for others, reflecting two phases of API development with different conventions.

### How to tell

Both patterns are applied broadly (not just one or two instances). Both have been maintained and extended by the team. If you asked "which is the standard?", reasonable people would disagree. There's no clear older/newer — both are actively used.

The key distinction from accidental variation: in a precedent conflict, each pattern was applied deliberately and consistently within its scope. In accidental variation, there's no consistency within either scope.

### How to resolve

Someone has to decide. Present both patterns to the user with the tradeoffs of each. Factor in:

- **Coverage** — which pattern is used in more places? Migration cost is lower for the less-used one.
- **Context fit** — does one pattern work better for the product's current direction?
- **User testing** — if the product has usage data, does one pattern perform better?
- **Migration cost** — is one pattern significantly harder to implement broadly?

Once decided, standardize the losing pattern to match the winner and document the canonical choice in CONTEXT.md. Consider an ADR if the decision is significant enough that someone might re-open it later.

---

## Ambiguous cases

Sometimes an inconsistency straddles two types. Common ambiguities:

**Accidental vs. drift:** Developer B didn't know about developer A's pattern (accidental), but developer A's pattern was also outdated (drift). Resolution: update to the newest intended pattern, not to either existing version.

**Intentional vs. conflict:** One team thinks the variation is intentional (different contexts), another thinks it's a conflict (same context, should be consistent). Resolution: ask whether the user sees the contexts as different. If they navigate between both in a single session and the difference would surprise them, it's a conflict regardless of intent.

**Drift vs. conflict:** The old pattern and new pattern both still have advocates. Resolution: this is a conflict. The fact that one predates the other doesn't make it automatically wrong — the team may have had reasons to keep both. Treat it as a decision to make, not a migration to complete.

When in doubt, classify as accidental variation and recommend standardization. The worst case is that the user tells you the variation was intentional, and you document it instead. The alternative — assuming intentionality and leaving a real inconsistency in place — is worse.
