# CONSISTENCY-DIMENSIONS.md

The seven dimensions of consistency. Each one is a different axis along which a product can drift. Checking all seven is the difference between catching the obvious naming problems and catching the subtle pattern problems that make a product feel like it was built by strangers who never talked to each other.

## 1. Naming

Same concept, same name, everywhere.

### What to look for

Every entity in the product has a name — or should have exactly one. Naming inconsistency happens when the same concept is called different things in different places. The user sees "Workspace" in the nav, "Space" in the settings, and "Team" in the docs, and they don't know if these are three things or one thing with an identity crisis.

### How to find it in code

- Grep for the primary noun of each entity. Then grep for plausible synonyms. If the product has "Projects," also search for "Workspace," "Space," "Folder," "Collection," "Group."
- Compare: nav labels, page titles, breadcrumbs, button labels (e.g. "New Project" vs. "Create Workspace"), error messages, empty state copy, settings labels, tooltips, documentation, onboarding copy.
- Check plural/singular consistency: is it "1 Project" and "3 Projects," or does one place say "Project(s)"?
- Check abbreviations: does one place say "Org" while another says "Organization"?
- Check capitalization: is it "project" in running text but "Project" in nav? Is that intentional (proper noun treatment) or accidental?

### What good looks like

One canonical name per concept. Used in every context — UI, docs, error messages, API responses surfaced to users. Plural form follows English rules consistently. Abbreviations are either always used or never used, not mixed.

### What bad looks like

The same entity has two or more names with no pattern to when each is used. Users encountering both in the same session can't tell if they're looking at the same thing. New team members can't tell either, which means the drift accelerates.

### Severity

**High.** Naming inconsistency directly confuses users. It's also the cheapest to fix — it's copy changes, not refactors. High impact, low effort. Fix these first.

---

## 2. Interaction

Same action, same mechanism, everywhere.

### What to look for

Every action in the product has a pattern — how it's triggered, what confirmation it requires, how it reports success or failure. Interaction inconsistency happens when the same class of action works differently in different places. Delete is an undo-toast here, a confirmation modal there, and no confirmation at all in a third place. None of these is wrong in isolation — but when they coexist without reason, the user can't build a mental model of how the product behaves.

### How to find it in code

- Map every destructive action (delete, remove, archive, revoke, disconnect). For each: what's the confirmation pattern? Toast, modal, inline confirm, nothing?
- Map every create action (new, create, add, invite, import). For each: inline creation, modal form, full-page form, wizard?
- Map every edit action (edit, update, rename, change). For each: inline edit, modal, navigate to edit page? Save explicitly or auto-save?
- Map every navigation trigger. For each: is it a link (`<a>`) or a button? Does it open in the same context or navigate away?
- Search for `confirm(`, `window.confirm`, modal/dialog components, toast/notification calls. Count the patterns.

### What good looks like

All destructive actions follow the same confirmation pattern (or have documented reasons for exceptions). All create actions use the same entry point pattern. Edit patterns are consistent by entity weight — lightweight entities edit inline, heavyweight entities navigate to an edit page, and the dividing line is clear.

### What bad looks like

Each screen invented its own interaction patterns independently. Delete uses three different confirmation mechanisms for entities of similar weight. Some forms auto-save, others require explicit save, with no logic to which is which. The user can't predict what will happen when they click.

### Severity

**High.** Interaction inconsistency erodes the user's trust in their own understanding of the product. They hesitate before acting because they don't know what the product will do. This is the consistency dimension that most directly impacts perceived reliability.

---

## 3. Visual

Same component, same appearance.

### What to look for

Components that are supposed to be the same but look subtly different. Cards with three different border radiuses. Buttons at four different sizes. Spacing that's 16px in one place and 20px in another. Icon styles that mix outlined and filled. This is not about whether a design system exists — it's about whether the same component actually renders the same way everywhere it appears.

### How to find it in code

- Grep for component usage (e.g. every instance of `<Card`, `<Button`, `<Modal`). Compare the props passed. Are some getting `size="sm"` and others `size="small"` and others no size at all?
- Search for inline styles and one-off CSS that overrides component defaults. Look for `style=`, `sx=`, `className=` with custom values on standard components.
- Search for duplicated components — files named similarly (`Card.tsx` and `CardNew.tsx` and `SimpleCard.tsx`) that do roughly the same thing with slight visual differences.
- Compare spacing values: search for padding/margin values across components. Are they using a consistent scale (4, 8, 12, 16, 24, 32) or arbitrary values (13, 17, 22)?
- Check icon usage: are icons from the same set? Mixed stroke widths? Inconsistent sizing?

### What good looks like

Components render identically in every context unless the context justifies a difference (compact mode, mobile, etc.). Spacing follows a scale. Colors come from tokens, not hex codes scattered through the codebase. When two things look the same, they are the same component — not two components that happen to look similar.

### What bad looks like

The product has three modal components, each with slightly different animations and widths. Cards have different shadows in different sections. Button labels use three different font weights. The overall impression is that every screen was designed independently, even if they share a component library.

### Severity

**Medium.** Visual inconsistency rarely confuses users about function, but it makes the product feel unpolished and uncoordinated. It signals that no one is looking at the product as a whole. Designers notice immediately; users feel it without naming it.

---

## 4. Copy

Same voice, same patterns.

### What to look for

The product's words should sound like they were written by the same person (or team with shared standards). Copy inconsistency happens when tone, formality, punctuation, and phrasing patterns vary across the product. Error messages are formal and technical in one section but casual and friendly in another. Buttons say "Save" here, "Submit" there, "Apply" elsewhere, and "Done" in a fourth place — all for the same action.

### How to find it in code

- Collect all button labels. Group by action type (save, cancel, delete, create, confirm). Do they use the same word for the same action?
- Collect all error messages. Compare tone: are some blaming ("Invalid input") and others gentle ("That doesn't look right")? Are some formal ("An error has occurred") and others casual ("Oops, something broke")?
- Collect all empty state copy. Compare structure: do some offer an action while others just describe the absence?
- Check punctuation patterns: exclamation marks in some places but not others? Periods on button labels in some places? Ellipsis usage?
- Check capitalization: Title Case in some places, Sentence case in others, ALL CAPS in a third?
- Check formality: "You" vs. "your" vs. impersonal. "Can't" vs. "Cannot." "Don't" vs. "Do not."

### What good looks like

Button labels for the same action use the same word everywhere. Error messages follow the same structure (what happened, what to do). Tone is uniform — if the product is direct, it's direct everywhere; if it's warm, it's warm everywhere. Capitalization follows one rule. Punctuation follows one rule.

### What bad looks like

The product sounds like three different people wrote it on three different days in three different moods. "Save" and "Submit" and "Apply" all appear for commit-changes actions. Some errors say "Oops!" and others say "Error: INVALID_REQUEST." Some empty states are encouraging and others are blank. The user subconsciously feels that the product has no identity.

### Severity

**Medium-high.** Copy is the product's voice. Inconsistent copy is an inconsistent voice, and an inconsistent voice signals an inconsistent team. Users may not articulate it, but they feel the dissonance. Copy fixes are also among the cheapest — string changes, no refactoring.

---

## 5. State

Same states, same treatment.

### What to look for

Every screen and component that loads data needs to handle the same set of states: loading, empty, error, success, and sometimes disabled/pending. State inconsistency happens when the same state gets different treatments in different places. Loading is a spinner on one screen, a skeleton on another, and absent on a third. Errors show a red banner here but a modal there. Success is a toast in one flow and an inline message in another.

### How to find it in code

- Map loading treatments: search for spinner components, skeleton components, `isLoading` / `loading` / `pending` state variables. How does each screen show that data is being fetched? List every variant.
- Map error treatments: search for error boundary components, error state renders, `isError` / `error` variables. How does each screen show that something failed? Toast, banner, inline, modal, page-level?
- Map empty treatments: search for empty state components, zero-length checks, `isEmpty` / `noData` variables. How does each screen show that there's nothing to display? (Cross-reference with audit-empty-states if you need depth here.)
- Map success treatments: search for success toasts, success messages, success animations. How does each screen confirm that an action worked? Toast, inline message, redirect, nothing?
- Map disabled treatments: search for disabled buttons, disabled inputs, `isDisabled` / `disabled` props. Do disabled elements explain why they're disabled?

### What good looks like

Loading uses one treatment for page-level loads and one for component-level loads, consistently. Errors follow a pattern by severity (inline for field errors, toast for transient errors, page-level for fatal errors). Success confirmations follow a pattern by action weight (toast for lightweight, redirect for heavyweight). The user can predict how the product will communicate state because it's been consistent about it.

### What bad looks like

The same screen has a spinner for one data source and a skeleton for another, with no reason for the difference. Some destructive actions show success toasts, others show nothing — the user deleted something and can't tell if it worked. Error handling is a patchwork: some screens have error boundaries, others crash, others silently show empty data.

### Severity

**High.** State inconsistency is a reliability signal. When loading, error, and success states aren't consistent, users can't trust their understanding of whether the product is working. A missing success confirmation after a destructive action is particularly damaging — the user can't tell if the action took effect.

---

## 6. Flow

Similar tasks, similar patterns.

### What to look for

Multi-step tasks that are structurally similar should follow similar patterns. Flow inconsistency happens when the same type of task (creation, editing, deletion, onboarding) is structured differently in different areas. Creating entity A is a three-step wizard, but creating entity B (which collects similar information) is a single page form. Editing an entity is a settings page with fields in a different order than the creation wizard. The user who learned one flow can't transfer that knowledge to the analogous flow.

### How to find it in code

- Map every creation flow: what steps, what fields, what order, what validation timing (on submit vs. on blur vs. on change)?
- Map every editing flow: are the same fields present as in creation? Same order? Same grouping? Same validation?
- Compare creation vs. editing for the same entity: is editing just creation pre-filled, or is it a completely different UI?
- Map every deletion/removal flow: how many confirmation steps? What information is shown before confirming? Is there an undo?
- Map onboarding and setup flows: step count, progress indication, ability to skip, ability to go back.
- Look for wizard components vs. single-page forms. When is each used, and is the dividing line consistent (e.g. "wizards for flows with more than 3 fields")?

### What good looks like

Similar entities have similar creation flows. Editing looks like creation-with-data, not a different UI. Field order is consistent between creation and editing. Validation timing is uniform. When the product uses wizards, there's a clear and consistent reason (complexity, required sequential decisions). The user who creates one type of entity can predict how creating another type will work.

### What bad looks like

Creation flows vary wildly by entity for no discernible reason. Editing re-arranges fields from the creation order. One flow validates on blur, another on submit. The user can't predict how any new flow will behave because no two existing flows work the same way. This is particularly common in products built by multiple teams without shared flow conventions.

### Severity

**Medium.** Flow inconsistency adds learning cost to every new task. Users who mastered one flow still feel like beginners when they encounter an analogous flow that works differently. It's more expensive to fix than naming or copy (it may require refactoring entire pages), so it's usually prioritized after the cheaper wins.

---

## 7. Terminology

Internal language aligned with user-facing language.

### What to look for

The names used in code (variable names, model names, API fields, database columns) should align with the names users see in the UI. Terminology inconsistency happens when the internal model says "organization" but the UI says "team" and the API says "account." This causes problems in two directions: developers build features using the internal name and forget to translate, and users see the internal name leak through in error messages, URLs, or API responses.

### How to find it in code

- Compare model/type names with UI labels. If the database table is `organizations` but the UI says "Teams," that's a gap.
- Compare API field names with display names. If the API returns `org_id` but the UI says "Team ID," that's a gap.
- Search for internal names appearing in user-facing strings. Grep for model names in string literals, error messages, toast messages, and UI copy.
- Check URL slugs: does the URL say `/organizations` while the nav says "Teams"?
- Check error messages generated from code: do validation errors reference internal field names ("organization_name is required") rather than user-facing names ("Team name is required")?
- Compare documentation/help content with UI labels. Do the docs use different terminology than the product itself?

### What good looks like

UI labels and code names are either identical or have a clear, documented mapping. Error messages use user-facing names, never internal names. URLs use the same terms as the UI. Documentation matches the product exactly. A developer can look at a UI label and find the corresponding code without guessing synonyms.

### What bad looks like

Internal names leak into the UI through error messages, URL paths, and auto-generated labels. The docs team uses different terms than the product team. Users see "organization" in an error message and have no idea what it maps to in the UI because the UI calls it a "team." Developers spend time translating between three or four names for the same concept.

### Severity

**Medium.** Terminology leaks confuse users when they encounter them and confuse developers all the time. The fix is usually a mapping layer (display name vs. internal name) plus a pass through error messages and auto-generated strings. Not as impactful as naming or interaction consistency for users, but often the cheapest win for developer experience.

---

## 8. Button Styling

Same button type, same visual treatment.

### What to look for

Button styling is the single most common source of visual drift in component-driven apps. Even with a well-defined `<Button>` component and a cva/variant system, individual pages accumulate one-off overrides — custom heights, padding, border radii, colors, and transitions that bypass the component's design tokens. The result is buttons that are technically the same component but look subtly different across screens.

This dimension gets its own pass because button inconsistencies are high-frequency, high-visibility, and cheap to fix.

### How to find it in code

**Step 1: Read the Button component definition.** Before flagging anything, understand the canonical variants, sizes, base styles, and transitions. Know what `default`, `outline`, `ghost`, `destructive` look like. Know the size scale (`default`, `sm`, `lg`, `xl`, `icon`). Know the base classes (border-radius, transition, focus ring).

**Step 2: Scan every page and component for these patterns:**

1. **Raw `<button>` elements acting as CTAs.** Search for `<button` with `className=` that includes color or sizing classes. If it functions as a CTA (create, submit, navigate, save), it should use the `<Button>` component or match its styles exactly. Common smell: `bg-blue-600 text-white rounded hover:bg-blue-700` — a raw button styled from scratch instead of using the component.

2. **CTA color drift.** Search for `bg-blue-600`, `bg-slate-900`, `bg-gray-900`, `bg-indigo-600`, or any non-canonical color on primary action buttons. The canonical primary CTA color is defined in the Button component — find it and enforce it everywhere. Also flag inline `style={{ backgroundColor: ... }}` on any button.

3. **Border-radius overrides.** If the Button component base sets `rounded-xl`, search for `rounded-lg`, `rounded-md`, `rounded-sm`, or bare `rounded` on any `<Button>` or `<button>` that acts as a CTA. These bypass the design system.

4. **Size overrides.** If the component defines sizes like `h-10 px-4` (default) and `h-8 px-3` (sm), flag any one-off heights like `h-9`, `h-7`, `h-6` or custom padding like `px-2`, `px-5`, `py-1` on buttons — unless the button is an icon-only button (`size="icon"`).

5. **Transition overrides.** If the base uses `transition-all duration-200`, flag any `transition-colors` or missing transition on buttons. This causes inconsistent hover/focus animation behavior.

6. **Create/Add button text patterns.** Decide on one canonical pattern for create-action wording. Common choice: `Action Text +` with a unicode plus at end, no icon. Flag any `<Plus icon> Text` or `Text <Plus icon>` patterns if the standard is unicode, or vice versa. The key is consistency — pick one and enforce it everywhere.

7. **Default variant misuse on primary CTAs.** The Button component's `default` variant is typically a dark neutral color. If the app's primary CTA convention uses a brand/accent color (e.g. violet), then a `<Button>` with no explicit variant or `variant="default"` used as the main action on a page/card/modal/empty-state is a mismatch — it should use the accent color explicitly.

### What good looks like

Every button that serves the same purpose looks identical. Primary CTAs use the same color, radius, height, padding, and transition everywhere. Create buttons use the same text pattern. Raw `<button>` elements don't exist as CTAs — they only appear for truly custom interactive elements (dropdown triggers, toggle handles) where the Button component's defaults would interfere. No inline style overrides on any button.

### What bad looks like

The same page has a `bg-violet-600` CTA button next to a `bg-gray-900` default-variant button and neither looks wrong in isolation, but together they signal two competing conventions. Another page uses raw `<button className="px-4 py-2 bg-blue-600 rounded">` because someone copied from a tutorial. A third page has `h-9 px-4 rounded-lg` because someone tried to split the difference between the `sm` and `default` sizes. Each button looks fine alone; together they make the product feel like a patchwork.

### Severity

**Medium-high.** Buttons are the most interacted-with elements in any UI. Users see dozens per session. Inconsistent buttons don't block functionality, but they visually fragment the product more than almost any other component. The fix is almost always a className change — no refactoring, no behavior change, no risk. High visibility, low effort. Fix these early.
