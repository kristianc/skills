# SMELL-TAXONOMY.md

The categories of structural problems this skill checks for. Organized by class, with what each looks like (specific patterns to search for), why AI produces it, severity, and the one-line fix direction. This is the reference the scan step (Step 2) walks through.

---

## 1. Structural smells (the architecture is wrong)

The code has no separation between concerns. Responsibilities that should live in different layers are tangled together in monolithic units.

### God components

**What it looks like:** A single component or file exceeding 300 lines that handles rendering, state management, API calls, business logic, and sometimes routing. Multiple `useState` calls (10+), multiple `useEffect` calls (5+), inline fetch calls, conditional rendering for completely different views.

**How to find it:** Sort files by line count — the top 20 are candidates. Look for files with more than 5 imports from different domains (UI library + API client + router + state + utils). Count `useState`/`useEffect` in React files; count methods in class files.

**Why AI produces it:** AI generates complete solutions in one shot. When prompted "build a dashboard that shows user stats, allows filtering, and updates in real time," it produces one file that does all three because that is the fewest-file solution to the prompt.

**Severity:** High. God components cannot be tested in isolation, cannot be reused, and break in unpredictable ways when modified.

**Fix direction:** Extract hooks (state + effects), extract services (API + business logic), extract sub-components (rendering). See REFACTORING-PLAYBOOK.md.

---

### No separation of concerns

**What it looks like:** API calls (`fetch`, `axios`) inside UI components. Business logic (price calculation, permission checks, data transformation) mixed with rendering. Formatting functions defined inline next to database queries. The "where does this live?" question has no consistent answer.

**How to find it:** Grep for `fetch(` or `axios.` inside component directories. Look for business logic keywords (calculate, validate, transform, filter, sort) inside files that also contain JSX/HTML/template markup. Check if there is any `services/`, `lib/`, `utils/`, or `api/` directory — if not, concerns are probably mixed.

**Why AI produces it:** AI optimizes for "fewest files to solve the problem," not "clearest boundaries between responsibilities." When asked to "add a feature that fetches user data and displays it," it puts both in the same file because that is the simplest complete answer.

**Severity:** High. Without separation, changes to business logic require touching UI code, changes to API contracts require touching every component that calls them, and nothing is testable in isolation.

**Fix direction:** Establish a layering convention (components -> hooks -> services -> API client) and migrate one feature at a time.

---

### Circular dependencies

**What it looks like:** Module A imports from B, B imports from C, C imports from A. Manifests as: unexpected `undefined` at runtime, import order sensitivity, bundler warnings about circular references, or mysterious bugs that appear only in production builds.

**How to find it:** Run `madge --circular` (JavaScript/TypeScript) or equivalent. Look for files that import from each other. Check for barrel files (`index.ts`) that re-export everything — these often create accidental cycles.

**Why AI produces it:** AI does not maintain a dependency graph across sessions. Each prompt produces a self-contained solution that imports what it needs without considering whether that creates a cycle with previously generated code.

**Severity:** Medium. Circular dependencies cause subtle bugs (undefined values at import time) and make the codebase harder to reason about, but they do not always break immediately.

**Fix direction:** Extract shared types/interfaces into a leaf module that both sides import from. Break the cycle by inverting the dependency (dependency injection, events, or a shared interface).

---

### Mixed abstraction levels

**What it looks like:** A single function that both parses raw HTTP response bytes AND formats dates for display. A method that validates user input, queries the database, sends an email, and returns a rendered template. You cannot describe what the function does in one sentence without using "and."

**How to find it:** Look for functions longer than 50 lines. Check if a single function references both low-level concerns (HTTP, file system, database cursors) and high-level concerns (business rules, UI formatting, user-facing messages). Count the number of distinct "topics" in a function.

**Why AI produces it:** AI solves the immediate need end-to-end. When asked "handle the form submission," it generates one function that validates, saves, sends notifications, and returns the response — because that is the complete solution to the prompt.

**Severity:** Medium. Mixed levels make code hard to test (you cannot test the business logic without also setting up HTTP mocks) and hard to reuse (you cannot use the validation without also triggering the email).

**Fix direction:** Extract each abstraction level into its own function. A high-level orchestrator calls lower-level functions. Each function operates at one level.

---

### Inconsistent patterns

**What it looks like:** Three different state management approaches in the same app (Redux in one feature, Zustand in another, raw useState in a third). Two different API calling conventions. React Query in some components, raw useEffect+fetch in others. Tailwind in half the files, CSS modules in the other half.

**How to find it:** Grep for competing libraries: `useQuery` vs `useEffect.*fetch`, `useSelector` vs `useContext` vs `useState` for global state, multiple styling approaches. Check if similar features are built with different tools.

**Why AI produces it:** Different prompts on different days produce different solutions. Monday's prompt uses React Query because the user mentioned it. Tuesday's prompt uses raw fetch because the user did not. Each individual solution is fine; the aggregate is inconsistent.

**Severity:** Medium. Inconsistency increases cognitive load ("which pattern does this part use?"), makes onboarding harder, and means bug fixes and improvements must be applied multiple times in multiple ways.

**Fix direction:** Pick the best pattern for each concern, document it, and migrate incrementally. Do not try to unify everything at once.

---

## 2. Duplication smells (DRY violations that AI loves)

Code that is repeated rather than shared. AI generates complete solutions from scratch each time, producing near-identical code in multiple places.

### Copy-paste components

**What it looks like:** `UserCard.tsx` and `TeamMemberCard.tsx` with 90% identical code, differing only in two props and a background color. `Modal.tsx` and `Dialog.tsx` and `Popup.tsx` that are the same component with different names. Multiple button components that vary only in size or color.

**How to find it:** Look for files with similar names (Card, Modal, Dialog, Button variants). Diff files that seem to do the same thing. Check for components that accept a `type` or `variant` prop that determines almost everything — these were probably consolidated from copies.

**Why AI produces it:** When asked "create a card for team members," AI generates a complete new component rather than parameterizing the existing UserCard. Each prompt is a fresh start; AI does not refactor existing code to accommodate new requirements unless explicitly told to.

**Severity:** High. When a bug exists in the shared logic, it must be found and fixed in every copy. When the design changes, every copy must be updated. Copies drift over time as different ones get different fixes.

**Fix direction:** Extract the shared structure into a base component with props/slots for the varying parts. Replace copies with instances of the shared component.

---

### Repeated API patterns

**What it looks like:** The same fetch-parse-error pattern duplicated across 15 files. Each API call manually sets headers, handles the response, parses JSON, catches errors, and manages loading state — identically.

**How to find it:** Grep for `fetch(` or `axios.get(` or `axios.post(` and count occurrences. If there are more than 10 raw fetch calls with similar structure, there is no shared client. Check if each call sets the same headers (Authorization, Content-Type) manually.

**Why AI produces it:** AI generates complete, self-contained code per prompt. "Fetch the user's orders" produces a full fetch call with headers, parsing, and error handling — not a call to a shared `apiClient.get("/orders")`.

**Severity:** High. When the auth token format changes, or the API base URL moves, or you need to add request logging, you touch every file that makes an API call.

**Fix direction:** Create a shared API client that handles auth headers, base URL, response parsing, and error handling. Replace raw fetch calls with client methods.

---

### Config scattered as magic numbers

**What it looks like:** The same timeout value (30000) hardcoded in 6 places. The API base URL as a string literal in 12 files. Port 3000 in both the server config and the CORS origin. Retry count of 3 in every error handler. Page size of 25 in every pagination call.

**How to find it:** Grep for common magic numbers: port numbers (3000, 8080), timeouts (5000, 10000, 30000), page sizes (10, 20, 25, 50, 100). Grep for URL patterns (`http://localhost`, `https://api.`). Check if there is a config file or environment variable system — if not, config is scattered.

**Why AI produces it:** AI uses concrete values because they make the code immediately runnable. When generating "an API call with a 30-second timeout," it writes `timeout: 30000` inline rather than `timeout: config.apiTimeout` — because the config system may not exist yet.

**Severity:** Medium. Scattered config makes changes error-prone (miss one location and behavior diverges) and makes environment-specific configuration impossible.

**Fix direction:** Centralize into environment variables or a config module. Replace magic numbers with named constants. A single source of truth for each value.

---

### Near-duplicate logic

**What it looks like:** Three functions that sort and filter data with slight variations (one filters by status, one by date, one by both — but the core sorting and pagination logic is identical). Multiple validation functions that check the same rules in slightly different orders.

**How to find it:** Look for functions with similar names (`filterUsers`, `filterOrders`, `filterProducts`). Search for repeated algorithmic patterns (sort + filter + paginate). Check utility files for functions that do almost the same thing.

**Why AI produces it:** Each prompt produces a complete implementation. "Filter orders by date" generates a new function rather than extending the existing filter utility with a date parameter. AI does not notice that the structure already exists elsewhere.

**Severity:** Medium. Near-duplicates are harder to spot than exact copies, so they drift further apart over time. Bugs fixed in one are not fixed in others.

**Fix direction:** Extract the shared algorithm into a parameterized function. Pass the varying parts (filter predicates, sort keys) as arguments.

---

## 3. Robustness smells (works on happy path only)

Code that assumes everything will succeed. No error states, no loading states, no edge cases. The code works in the demo; it breaks in production.

### No error handling

**What it looks like:** `fetch()` without `.catch()`. `async/await` without `try/catch`. No error boundaries in React. API calls that assume 200 responses. JSON parsing that assumes valid JSON. Array access that assumes elements exist.

**How to find it:** Grep for `await` not inside a try block. Grep for `.then(` without a corresponding `.catch(`. Look for React apps without `ErrorBoundary` components. Check if the global error handler returns something useful or just crashes.

**Why AI produces it:** AI generates the happy path because that is what the prompt asks for. "Fetch user data and display it" does not mention errors, so the generated code does not handle them. The code satisfies the prompt without handling any failure mode.

**Severity:** Critical. Missing error handling means silent failures in production — data disappears, actions fail without feedback, and the user does not know what happened.

**Fix direction:** Add error handling at boundaries (API calls, user input, external dependencies). Introduce error boundaries for UI. Handle at the call site or propagate explicitly. See REFACTORING-PLAYBOOK.md.

---

### No loading states

**What it looks like:** Data is either `undefined` or loaded. Components render empty during the fetch, then flash into existence. Buttons do not disable during submission. No skeletons, no spinners, no optimistic UI — just a blank gap.

**How to find it:** Look for data fetching without a corresponding `isLoading` state. Check if components handle the undefined/null case or just assume data is present. Search for form submissions without disabled state on the submit button.

**Why AI produces it:** Loading states are separate from the "working" state. AI generates the end state — the component with data rendered — because that is the visible goal of the prompt. The transition is invisible in code prompts.

**Severity:** Medium. Missing loading states cause perceived performance issues, double-submissions, and jarring layout shifts. Not broken, but unprofessional and confusing.

**Fix direction:** Add loading/error/success states to every async operation. Use skeleton screens for initial loads, inline indicators for actions.

---

### No empty states

**What it looks like:** Lists that render nothing (blank space) when the array is empty. Tables with headers but no rows and no message. Dashboards that show zeroes everywhere for new users with no explanation.

**How to find it:** Look for `.map()` calls on data arrays without a corresponding empty check. Search for list/table components and check if they handle the zero-items case. Check the first-run experience — what does a new user see?

**Why AI produces it:** AI generates components with data. The example data in the prompt always has items. Empty state is never part of the "build me a list" prompt, so it is never part of the output.

**Severity:** Low to medium. Empty states are a polish issue more than a structural one, but they signal that edge cases are not considered anywhere.

**Fix direction:** Add explicit empty state rendering for every list, table, and collection. Use it as an opportunity to guide the user ("No orders yet. Create your first order.").

---

### No validation at boundaries

**What it looks like:** User input from forms passed directly to API calls without validation. API response data used without type checking or shape verification. URL parameters used as-is without sanitization. File uploads accepted without size or type checking.

**How to find it:** Trace user input from form to API call — is anything validated in between? Check if API responses are typed (TypeScript) or assumed (JavaScript). Look for `req.body.whatever` or `req.params.id` used without validation.

**Why AI produces it:** Validation is boilerplate that does not make the demo work. AI generates the data flow (input -> process -> output) without the validation layer because the prompt does not mention invalid input.

**Severity:** High. Missing validation causes runtime crashes on unexpected input, security vulnerabilities (injection), and data corruption.

**Fix direction:** Add validation at every boundary: user input (forms), API boundaries (request/response), and system boundaries (file I/O, external services). Use zod, yup, or equivalent.

---

### Optimistic with no rollback

**What it looks like:** UI state updated immediately on action (optimistic update) but no mechanism to revert if the API call fails. Item removed from list before the delete confirms. Counter incremented before the server acknowledges. The user sees success, then nothing happens.

**How to find it:** Look for state mutations that happen before the corresponding API call completes. Check if `.catch()` or error handlers restore the previous state. Search for "optimistic" in comments without corresponding rollback code.

**Why AI produces it:** Optimistic updates are a known good pattern, and AI generates them when prompted for responsive UIs. But the rollback path is easy to forget because it is the error case — which AI chronically under-handles.

**Severity:** Medium. Users experience phantom actions — things they did that did not actually happen. Confusing and trust-eroding.

**Fix direction:** For every optimistic update, store the previous state and restore it on failure. Show a toast or notification when rollback occurs.

---

### No timeouts on external calls

**What it looks like:** HTTP requests with no timeout configuration. Database queries that can run indefinitely. WebSocket connections that never time out. Third-party API calls that hang forever if the service is slow.

**How to find it:** Check HTTP client configuration for `timeout` settings. If using `fetch`, check for `AbortController` with timeout. Look at database connection config for statement timeouts. Grep for any external call and check if timeout is configured.

**Why AI produces it:** Timeouts are infrastructure concerns that do not make the demo work. The prompt "call the weather API" does not mention timeouts, and the API works fine in development, so the code works without them.

**Severity:** Critical. A hung external call without a timeout consumes a connection/thread indefinitely. Under load, this exhausts the connection pool and brings down the entire service.

**Fix direction:** Set explicit timeouts on every external call. Use AbortController for fetch, timeout config for axios/libraries, statement_timeout for databases.

---

## 4. Maintenance smells (cannot be changed safely)

Code that works but fights you when you try to modify it. The cost of change is disproportionate to the change itself.

### TODO-as-architecture

**What it looks like:** `// TODO: add auth`, `// TODO: handle errors`, `// TODO: make this configurable`, `// FIXME: this is a hack` — comments that represent missing functionality, not future improvements. The TODO is load-bearing: without the thing it describes, the code is incomplete.

**How to find it:** Grep for `TODO`, `FIXME`, `HACK`, `XXX`. Count them. Read the ones in critical paths (auth, payment, data handling). Distinguish between aspirational TODOs ("TODO: add dark mode support") and structural TODOs ("TODO: add error handling for API failures").

**Why AI produces it:** AI uses TODOs as escape hatches when it cannot fully solve a problem in context. "Build a payment flow" might produce working Stripe integration with `// TODO: handle webhook verification` — because that requires architecture the AI cannot infer from the prompt.

**Severity:** High (for structural TODOs). Each one is a known gap that will cause a production issue. They accumulate because they are easy to skip past.

**Fix direction:** Triage TODOs into "must fix" (security, error handling, data integrity) vs. "nice to have" (features, optimizations). Fix the must-fix ones. Delete or convert nice-to-haves into tracked issues.

---

### No types / `any` everywhere

**What it looks like:** TypeScript files full of `: any`, `as any`, `@ts-ignore`, `@ts-expect-error`. Function parameters untyped. API response types as `any`. Props interfaces that use `Record<string, any>`. The type system provides no safety — it is just syntax.

**How to find it:** Grep for `: any`, `as any`, `@ts-ignore`, `@ts-expect-error`. Count them relative to the codebase size. Check the most-used interfaces and types — are they specific or generic? Look at API response handling — is the data typed?

**Why AI produces it:** AI uses `any` to make code compile without fully understanding the data shapes. When the type is complex or unknown, `any` is the path of least resistance. It satisfies TypeScript without providing value.

**Severity:** Medium. `any` erodes the type system incrementally. Each `any` is a place where refactoring cannot be verified by the compiler and runtime errors can hide.

**Fix direction:** Type the most-used interfaces first (API responses, shared props, state shapes). Replace `any` incrementally, starting with the modules that are imported most. See REFACTORING-PLAYBOOK.md.

---

### Dead code

**What it looks like:** Unused functions exported but never imported. Commented-out blocks of 50+ lines. Components that exist but are not rendered anywhere. Imports at the top of files that are not used. CSS classes that match no elements. API endpoints that no client calls.

**How to find it:** Run the TypeScript compiler with `noUnusedLocals` and `noUnusedParameters`. Check for ESLint unused-vars warnings. Search for commented-out blocks (lines starting with `//` or `/* */` spanning more than 5 lines). Use `ts-prune` or `knip` to find unused exports.

**Why AI produces it:** AI generates code for each prompt independently. When requirements change (user says "actually, use a modal instead of a page"), the old code is not removed — it is commented out or left in place. AI also generates utility functions "just in case" that are never used.

**Severity:** Low. Dead code is noise, not danger. It increases cognitive load and file sizes but does not cause bugs. However, large amounts of dead code signal that no one is maintaining the codebase.

**Fix direction:** Delete it. If it is in git, it can be recovered. Commented-out code is not a backup; it is clutter.

---

### Console.log as observability

**What it looks like:** `console.log("here")`, `console.log(data)`, `console.log("user:", user)` scattered through production code. No structured logging. No log levels. No way to filter or search logs. Debug output that was useful during development and never removed.

**How to find it:** Grep for `console.log`, `console.warn`, `console.error`. Count them. Check if there is a proper logging library configured. Look at whether console statements are in production code paths or just test files.

**Why AI produces it:** AI uses console.log for immediate feedback during development. When building iteratively with AI, each round adds logs to verify behavior. They are never cleaned up because the next prompt does not say "remove the console.logs from last time."

**Severity:** Low. Console logs in production are noisy and unprofessional but rarely dangerous. However, they can leak sensitive data if they log request bodies or user information.

**Fix direction:** Replace with a structured logger (pino, winston, etc.) that supports log levels and can be configured per environment. Remove debug-only logs. Keep intentional logs.

---

### No tests

**What it looks like:** No test files. Or test files that test implementation details ("it calls setState with the right value") instead of behavior ("it shows an error message when the API fails"). Tests that mock so aggressively that they test nothing. Zero tests for critical business logic.

**How to find it:** Check for test directories (`__tests__`, `*.test.*`, `*.spec.*`). Count test files relative to source files. Read the tests — do they test behavior or implementation? Check coverage of critical paths (auth, payment, data mutation).

**Why AI produces it:** Tests are rarely part of the initial prompt. "Build a checkout flow" does not produce tests unless explicitly requested. When tests are requested, AI tends to test the internals it generated rather than the external behavior, because it knows its own implementation intimately.

**Severity:** High. Without tests, refactoring is dangerous — you cannot verify that changes preserve behavior. The codebase ossifies because no one dares touch it.

**Fix direction:** Start with integration tests on critical paths (what happens when a user does X). Add unit tests for complex business logic. Do not aim for coverage numbers; aim for confidence in the critical paths.

---

### Massive dependencies

**What it looks like:** 50+ npm packages for a simple landing page. `moment.js` imported for one date format. `lodash` imported wholesale for one utility. Multiple packages that do the same thing (both `axios` and `node-fetch`, both `moment` and `date-fns`). Packages with 200+ transitive dependencies for trivial functionality.

**How to find it:** Count the packages in `package.json` (or equivalent). Check bundle size with `bundlephobia` for the largest ones. Look for packages that could be replaced by 5 lines of code. Check for duplicate functionality across packages.

**Severity:** Low to medium. Large dependency trees increase install time, bundle size, supply chain attack surface, and maintenance burden. They slow everything down without providing proportional value.

**Fix direction:** Audit and remove unused packages. Replace heavy packages with lighter alternatives or native implementations. Consolidate duplicates.

---

## 5. Convention smells (no consistent style)

The code has no internal consistency. Each file looks like it was written by a different person (because it was written by a different prompt).

### Naming chaos

**What it looks like:** `camelCase` mixed with `snake_case` in the same file. Single-letter variables (`x`, `d`, `e`) for non-trivial values. Function names that do not communicate intent (`handleClick`, `processData`, `doStuff`). Inconsistent naming for the same concept (`user`, `currentUser`, `loggedInUser`, `usr` all meaning the same thing).

**How to find it:** Scan variable names in key files. Look for single-letter variables outside of loop counters. Check if similar concepts have different names across files. Look for generic names (`data`, `result`, `temp`, `item`) that could be anything.

**Why AI produces it:** AI mirrors the style of the prompt. If the user writes `getUserData`, AI uses camelCase. If the next prompt says `get_user_data`, AI switches to snake_case. Each prompt is a fresh style context. AI also defaults to generic names when the domain is unclear.

**Severity:** Low. Naming inconsistency is annoying and slows comprehension but does not cause bugs. It matters more in shared/public APIs than in internal code.

**Fix direction:** Establish a naming convention (usually the language's idiom — camelCase for JS/TS, snake_case for Python/Ruby). Rename incrementally, starting with the most-visible exports.

---

### File organization chaos

**What it looks like:** No discernible structure — components, utils, pages, services all in the same directory. Or over-nested: `src/components/ui/buttons/primary/PrimaryButton/PrimaryButton.tsx` (five levels for one file). Barrel files that re-export everything. Files with 1-2 lines that exist only to re-export from somewhere else.

**How to find it:** Look at the top-level `src/` directory. Is there a clear convention? Count files in the flattest directories. Check for directories with a single file. Check for barrel files (`index.ts`) that contain only re-exports.

**Why AI produces it:** AI creates files where the prompt suggests. "Create a button component" might put it in `src/components/` or `src/ui/` or `src/shared/` depending on context. Without consistent prompting, the file tree reflects the sequence of prompts, not a deliberate architecture.

**Severity:** Low. File organization is irritating but does not affect runtime behavior. It matters most for onboarding — a new developer should be able to guess where a file lives.

**Fix direction:** Establish a convention (feature-based, type-based, or hybrid) and reorganize one feature at a time. Document the convention so future files go in the right place.

---

### Style mixing

**What it looks like:** Tailwind classes in some files, CSS modules in others, inline styles in a third. Multiple CSS-in-JS libraries. Global CSS conflicting with scoped styles. !important used to override specificity battles caused by multiple systems.

**How to find it:** Check for multiple styling approaches: `className=` with Tailwind classes, `import styles from`, `style={{`, `styled.div`, `@apply`. If more than one approach is used for the same type of component (not intentionally split between "app chrome uses CSS modules" and "content uses Tailwind"), it is mixed.

**Why AI produces it:** AI uses whatever styling approach the prompt or context suggests. If the example code uses Tailwind, the output uses Tailwind. If the next prompt has no example, AI may default to inline styles or CSS modules. Each prompt is styled in isolation.

**Severity:** Low. Style mixing creates maintenance burden (multiple mental models) and can cause specificity conflicts, but does not break functionality.

**Fix direction:** Pick one styling system and migrate incrementally. Start with new code (enforce the convention) and migrate old code as you touch it.

---

### Import disorder

**What it looks like:** No grouping of imports (React, third-party, local all mixed). Relative paths 8 levels deep (`../../../../../../../utils/format`). Barrel files that re-export 50 modules (slowing bundling and creating circular dependency risks). No path aliases configured.

**How to find it:** Look at the import blocks in key files. Are they grouped (external/internal/relative)? Are relative paths more than 3 levels deep? Are there barrel files with many re-exports? Check for path alias configuration (`tsconfig.json` paths, webpack aliases).

**Why AI produces it:** AI uses whatever import path resolves correctly. If the file is deeply nested, it generates deep relative paths. It does not configure path aliases or enforce import ordering because those are project-level concerns outside the scope of a single prompt.

**Severity:** Low. Import disorder is cosmetic. It makes files harder to scan and deep relative paths are fragile when files move, but it does not affect behavior.

**Fix direction:** Configure path aliases (`@/` for src root). Add ESLint import ordering rules. Fix the deepest relative paths first.
