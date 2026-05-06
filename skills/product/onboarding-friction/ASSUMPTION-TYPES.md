# ASSUMPTION-TYPES.md

A taxonomy of the hidden assumptions products make about first-time users. Every friction point in an activation path traces back to one of these. If you can name the assumption type, you can spot it faster, grade its severity more accurately, and choose the right remedy.

## Vocabulary assumptions

The product uses a term the user hasn't learned yet, and doesn't define it.

### What it looks like

A nav item, heading, button label, or empty state that uses a product-specific word without explaining what it means. The user sees "Workspaces" in the sidebar and doesn't know what a workspace is in this product. The onboarding says "Create your first Pipeline" and the user has never heard the word pipeline used this way.

### How to spot it

Read every noun in the UI as if you've never used the product. If a word requires context from the product's docs, marketing site, or prior use to understand, it's a vocabulary assumption. Pay special attention to:

- Navigation labels — the user reads these before anything else
- Empty states — the first encounter with a concept is often an empty list of them
- Form labels and placeholder text — "Slug," "Namespace," "Tenant"
- Error messages — "Invalid workspace configuration" assumes the user knows what workspace configuration means

### Concrete examples

- A project management tool whose sidebar says "Epics" with no explanation. The user who's never used agile methodology doesn't know what an epic is.
- A messaging app that says "Threads" in the nav. The user might think email threads, Slack threads, or forum threads — all different mental models.
- A button labeled "Hydrate" in a data tool. Internally coherent; externally opaque.

### Severity heuristics

- **Blocking** if the vocabulary is on a required step and the user can't guess the meaning from context. ("Create a Workspace" when you can't skip it and don't know what one is.)
- **Confusing** if the user can proceed but picks the wrong interpretation. ("Threads" that aren't what they think threads are.)
- **Cosmetic** if the term is unfamiliar but the surrounding UI makes the meaning clear enough to continue. (A "Dashboard" label on a page that's obviously a dashboard.)

## Workflow assumptions

The product relies on an interaction convention the user may not know — something they need to do, but the UI doesn't tell them how.

### What it looks like

A list that can be reordered by dragging, but nothing indicates it's draggable. A context menu that appears on right-click, but no visible affordance suggests right-clicking. A keyboard shortcut that's the only way to reach an action. A double-click that's distinct from a single-click, with no indication.

### How to spot it

For every interactive element, ask: "How would a user know they can do this?" If the answer is "they'd figure it out" or "it's a standard convention," that's a workflow assumption. Standard conventions vary wildly by user population — drag-to-reorder is obvious to Trello users and invisible to everyone else.

Check for:

- Drag interactions without drag handles or visual cues
- Right-click or long-press menus with no alternative access path
- Keyboard shortcuts as the only route to an action
- Double-click or hover behaviors that reveal critical UI
- Swipe gestures on mobile without visual affordance
- Multi-select behaviors (shift-click, cmd-click) used in core flows

### Concrete examples

- A kanban board where cards are draggable but show no drag handle. A user who doesn't try dragging will think the board is static.
- A table where clicking a column header sorts it, but nothing about the header suggests interactivity — no pointer cursor, no sort icon, no underline.
- An app where the only way to delete an item is via right-click menu. Users who don't right-click will think deletion is impossible.

### Severity heuristics

- **Blocking** if the hidden workflow is on the activation path and there's no alternative. (The only way to proceed is drag-to-reorder, and nothing suggests dragging.)
- **Confusing** if there's an alternative path but the hidden workflow is significantly faster or more discoverable users will use the slow path unnecessarily.
- **Cosmetic** if the convention is genuinely standard for the target audience. (Ctrl+S to save in a developer tool.)

## Mental model assumptions

The product's internal data model, hierarchy, or relationships are exposed in the UI without explanation, and the user needs to understand the structure to use the product.

### What it looks like

The product has a hierarchy — Organizations contain Workspaces, Workspaces contain Projects, Projects contain Tasks — and the UI navigates this hierarchy without ever explaining it. The user creates something at the wrong level because they don't understand the nesting. Or the product has a concept like "environments" (dev, staging, prod) that the user has never encountered.

### How to spot it

Look for:

- Nested navigation that implies a containment hierarchy (org > workspace > project)
- Breadcrumbs that show a structure the user hasn't been taught
- Creation flows that ask "where" before the user understands the "where" options ("Which workspace should this project live in?")
- Permissions or visibility scoped to a level the user doesn't understand ("This is visible to everyone in the workspace" — but the user doesn't know who's in the workspace or what a workspace boundary means)
- Relationships between objects that are implied but not explained ("Linked to Pipeline #4" — what is a pipeline, and what does linking do?)

### Concrete examples

- A product with Organizations, Teams, and Projects where a new user is asked "Create a Team" before they understand how Teams relate to their Organization or their Projects.
- An analytics tool that asks "Select an Environment" on first run, showing "Development," "Staging," and "Production" — meaningless to a user who's never deployed software.
- A CRM where "Contacts" belong to "Companies" belong to "Deals," and the UI shows this three-level hierarchy from day one without explaining why a contact would belong to a company.

### Severity heuristics

- **Blocking** if the user must choose correctly to proceed and the wrong choice is hard to undo. ("Choose an environment" where picking wrong means starting over.)
- **Confusing** if the user can proceed but builds on a misunderstanding that compounds. (Creating resources in the wrong workspace, which they'll discover later when permissions don't work.)
- **Cosmetic** if the hierarchy is visible but doesn't affect the user's immediate path. (Breadcrumbs that show org > workspace > project, but the user only has one of each.)

## Prior-action assumptions

The product expects the user to have already done something — configured a setting, created a prerequisite object, connected an integration — and the current step doesn't work or make sense without that prior action.

### What it looks like

A screen that says "No data yet" because the user hasn't connected a data source, but doesn't tell them that's why it's empty or how to connect one. A feature that's greyed out because a prerequisite isn't met, with no explanation of what's missing. A form that requires selecting from a list of items the user hasn't created yet.

### How to spot it

For every empty state, disabled control, and error message, ask: "Is this empty/disabled/broken because the user hasn't done something else first?" Then check whether the UI tells them what that something is.

Look for:

- Empty states that don't explain why they're empty or what action would fill them
- Disabled buttons or greyed-out features with no tooltip or explanation
- Dropdown menus that are empty because the user hasn't created the items yet
- Errors that say "X not configured" without explaining what X is or where to configure it
- Features that require an integration, API key, or external setup that the product doesn't guide the user through

### Concrete examples

- A dashboard that shows "No data" because the user hasn't installed the tracking snippet, but doesn't mention the snippet or link to setup instructions.
- A "Share with Team" button that's disabled because the user hasn't created a team yet, but the button just looks broken — no tooltip, no "Create a team first" message.
- A report builder that says "Select a data source" with an empty dropdown, because data sources are configured in a settings page the user hasn't visited.

### Severity heuristics

- **Blocking** if the user literally cannot proceed without the prior action, and the UI doesn't tell them what it is. (An empty dropdown with no explanation.)
- **Confusing** if the user can work around it but wastes time figuring out what's wrong. (A "No data" screen that eventually leads to a help article.)
- **Cosmetic** if the prior action is clearly communicated but slightly buried. (A disabled button with a tooltip that says "Connect a data source first.")

## Technical assumptions

The product assumes the user understands a technical concept — an API, a protocol, a data format, a system administration concept — that's not part of the product's own domain.

### What it looks like

The product asks the user to "Enter your API key" and the user doesn't know what an API key is, where to find one, or why the product needs it. The setup flow says "Configure your webhook URL" assuming the user knows what a webhook is. The error says "CORS error" or "401 Unauthorized."

### How to spot it

For every technical term in the UI, ask: "Does the target user know this?" The answer depends entirely on who the target user is. A developer tool can assume the user knows what an API key is. A marketing tool aimed at non-technical marketers cannot.

Check for:

- Setup flows that require API keys, tokens, or secrets without explaining what they are or where to find them
- Error messages that surface HTTP status codes, framework errors, or system messages
- Configuration that uses technical concepts (ports, protocols, headers, environment variables)
- Jargon leaks from the tech stack — "null," "undefined," "404," "timeout," "payload"
- Documentation links that point to API references when the user needs a how-to guide

### Concrete examples

- A marketing analytics tool that asks non-technical users to "paste your tracking snippet into the <head> of your site." The user doesn't know what a <head> tag is or how to edit their site's HTML.
- An integration setup that says "Enter your OAuth callback URL" for a user who's never heard of OAuth.
- An error message that reads "Request failed with status 429" instead of "You've made too many requests. Try again in a few minutes."

### Severity heuristics

- **Blocking** if the technical step is required and the user has no background to complete it. (A non-developer user asked to paste a code snippet.)
- **Confusing** if the technical concept is used in messaging but the user can still complete the task through a non-technical path. (An error that says "timeout" but the user can just retry.)
- **Cosmetic** if the technical language is incidental and the user can ignore it. (A URL visible in the address bar that contains "api/v2".)

## Compound assumptions

Friction points often combine multiple assumption types. A screen that asks "Select an Environment for your Pipeline" combines a mental model assumption (environments), a vocabulary assumption (pipeline), and possibly a prior-action assumption (the user may need to have created a pipeline first).

When mapping friction, note the primary assumption type — the one that hits first — but flag compounds. Compound assumptions are almost always more severe than single assumptions, because the user faces multiple unknowns simultaneously. A vocabulary assumption on its own might be cosmetic; the same vocabulary assumption stacked on top of a mental model assumption becomes confusing or blocking.

## Using this taxonomy

When walking the activation path (Step 2 of the process), check each step against all five types. Don't only look for the obvious ones — vocabulary assumptions are the easiest to spot; prior-action and mental model assumptions hide deeper but cause more damage.

When grading severity, the type matters. Vocabulary and workflow assumptions are often fixable with better labels and affordances. Mental model and prior-action assumptions may require reordering the activation path itself. Technical assumptions depend entirely on the target user — they're either cosmetic or blocking with very little middle ground.
