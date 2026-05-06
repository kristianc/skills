# LANGUAGE.md

The vocabulary the skill uses. The point of having a glossary is that "this feels off" stops being the working language. Once you can name what's off, you can decide what to do about it.

## Glossary

**Interaction** — a unit of user input and system response. Click, hover, submit, load, error, success, return-after-absence, idle timeout. Even "the user opens the app cold for the first time today" is an interaction.

**Touchpoint** — a specific moment within an interaction where craft is visible or absent. The same flow has many touchpoints: the click, the loading wait, the success state, the toast that fades, the return visit. Each one earns its place or doesn't.

**Friction** — cost the user pays beyond what the task requires. Time (waiting), attention (notifications, modals, banners), trust (errors that don't explain), effort (retyping, re-navigating, re-deciding). Not all friction is bad — required confirmation for an irreversible payment is friction earning its keep. The question is always whether the cost is paid for something.

**Affordance** — what a control communicates about itself before you use it. A button that looks pressable. A link that looks clickable. A drop zone that looks like a drop zone. Disabled controls have weak affordance unless they explain why they're disabled. Destructive controls have weak affordance when they look identical to safe ones.

**Feedback** — the system's response to an action, telling the user what happened. Good feedback is immediate (acknowledged within 100ms), specific (says what, not just that), and in the product's voice. "Saved" is feedback. "Operation completed successfully." is system noise dressed as feedback.

**Default** — the unconsidered version of a touchpoint. The browser's `alert()`. The blank empty state. "An error occurred. Please try again." The form that loses your input on validation failure. The literal string "Loading...". The 200ms transition someone copy-pasted from a tutorial. Defaults aren't always wrong, but they're always unconsidered.

**Crafted** — the considered version. Anticipates the user's situation (what brought them here, what they're trying to do), knows their likely next move (and surfaces it), uses the product's voice. Crafted does not mean ornate.

**Polish** — the gap between default and crafted. Also: the work of closing that gap.

**Voice** — the product's consistent way of speaking. Vocabulary, formality, attitude, what it apologizes for, whether it uses exclamation marks. Defined in CONTEXT.md.

**Pattern** — a touchpoint treatment that has been used at least twice and named. "We use undo toasts for destructive actions" is a pattern. "We thought about using an undo toast once" is not. Patterns live in CONTEXT.md.

**Anti-pattern** — an explicit decision about what the product does not do. "We don't use exclamation marks." "We don't use browser confirms." Equally load-bearing as patterns; lives in CONTEXT.md too.

## Key principles

### The articulation test

Point at any touchpoint and say, in user terms, what it earns. "It loads" doesn't count. "Confirms within 100ms so the user doesn't tap twice" does. "It shows an error" doesn't count; "it shows the user what they typed wrong and how to fix it" does. If you can't articulate, you can't improve — and the touchpoint is almost certainly a default.

### Subtraction beats addition

The best polish removes friction more often than it adds delight. A confirmation that becomes an undo. A wait that becomes optimistic. A three-step flow that becomes one. A modal that becomes inline. Reach for removal first; if you can't remove, then improve.

### Voice is part of UX

The button label is an interaction. The empty-state line is an interaction. The error copy is an interaction. Cheap copy makes a product feel cheap regardless of the layout. Treat words with the same care as pixels — and ideally, more, because words are easier to fix and harder to forgive.

### Feel is cumulative

Each touchpoint is small. The impression is the sum. Five defaults in a row reads as "this product doesn't care." One crafted touchpoint among many defaults reads as accident. Polish work is rarely about one heroic moment — it's about the rate at which considered moments outnumber unconsidered ones.

### Anticipation beats reaction

The crafted version of a touchpoint knows what brought the user here and what they'll likely do next. Empty states offer the action that fills them, not just describe the absence. Errors offer the recovery, not just the diagnosis. Loading states tell the user how long, when the system has any idea.

### Patterns earn their name through repetition

One bespoke treatment is a one-off. Two is a pattern — worth naming in CONTEXT.md so the rest of the product can stay consistent. Don't promote a single instance to a pattern (premature standardization is its own pollution); don't leave a real pattern unnamed (drift starts immediately).

### Forgiveness over confirmation

For most destructive actions, undo is better than "Are you sure?". Confirmation interrupts everyone to protect against the rare mistake; undo lets the common case stay fast. Reserve confirmation for the truly irreversible — payment, account deletion, public publish.

### Don't sound like the system

"Invalid input." "Operation failed." "Successfully completed." These are system messages dressed as UI. Replace them with sentences a person would say. The test: would a colleague leaning over your shoulder say this sentence out loud, or would they paraphrase it?

### Defaults compound

A default error message is fine in isolation. A default error message + default loading state + default empty state + default validation behaviour, all on the same screen, is not the sum of four small problems. It's the screen telling the user that no one was paying attention here. Polish in clusters.

## Words to avoid

When discussing polish opportunities, avoid these — they hide what you mean:

- **"Smooth," "clean," "intuitive," "modern," "polished"** — all vague. Say what specifically changes and why.
- **"UX" / "the experience"** — too broad. Name the touchpoint.
- **"Better"** without a comparison axis — better at what? Faster, more recoverable, more in voice, less interrupting?
- **"Delight"** — usually a euphemism for animation that no one asked for. If the change earns its place, articulate what it earns.
- **"User-friendly"** — friendly to which user, in which state, doing what?

If you find yourself reaching for one of these words, you haven't finished thinking. Push one level deeper and name the specific change.
