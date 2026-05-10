# REFACTORING-PLAYBOOK.md

The fixes, organized by smell category. For each: the refactoring move (specific steps), how to do it incrementally, how to verify it is safe, and common mistakes when refactoring AI code.

The golden rule: **never refactor something you do not have a test for (or cannot manually verify).** The worst outcome is a cleaner codebase that is also broken.

---

## Extract hook

**Addresses:** God components, mixed abstraction levels, no separation of concerns.

**The move:** Pull state management and side effects out of a component into a custom hook. The component becomes a pure rendering function; the hook encapsulates behavior.

**Steps:**
1. Identify the state variables and effects that belong together (e.g., everything related to fetching and managing user data)
2. Create a new hook file: `useUserData.ts`
3. Move the `useState` calls, `useEffect` calls, and handler functions into the hook
4. Return only what the component needs: `{ data, isLoading, error, refetch }`
5. Replace the extracted state/effects in the component with a single `const { data, isLoading, error } = useUserData()`
6. Verify: the component renders the same output given the same hook return values

**How to verify safely:**
- Before extracting, manually test the component's behavior (or run existing tests)
- After extracting, verify the same behavior — same renders, same API calls, same user interactions
- If the component has no tests, add a smoke test before refactoring that verifies the visible behavior

**Incremental approach:** Extract one hook at a time. A god component with 10 state variables might yield 3 hooks — extract the most tangled one first.

**Common mistakes:**
- Extracting too granularly — a hook with one `useState` and one `useEffect` is not worth the abstraction
- Creating hooks that depend on each other's internal state — this recreates the coupling at the hook level
- Extracting presentation logic into hooks — formatting and display logic belongs in the component or a utility function, not a hook

---

## Extract service

**Addresses:** No separation of concerns, API calls in components, business logic in UI.

**The move:** Pull API calls and business logic into a service module that the component (or hook) calls. The service knows nothing about UI; the component knows nothing about HTTP.

**Steps:**
1. Identify the API calls and business logic in the component
2. Create a service file: `services/userService.ts`
3. Move fetch calls into typed functions: `getUser(id: string): Promise<User>`
4. Move business logic into pure functions: `calculateDiscount(user: User, plan: Plan): number`
5. Import and call the service functions from the component/hook
6. Verify: the same API calls are made, the same data is returned

**How to verify safely:**
- Service functions are independently testable — add tests for them after extraction
- The component should behave identically before and after
- If you also change the API response handling (e.g., adding error mapping), do that in a separate step

**Incremental approach:** Extract one service per feature area. Do not try to build a universal service layer upfront.

**Common mistakes:**
- Over-abstracting — a service that wraps a single fetch call with no additional logic is just indirection
- Making services stateful — services should be collections of pure functions or thin wrappers around API calls, not state containers
- Creating a generic "ApiService" that does everything — keep services domain-specific

---

## Extract component

**Addresses:** God components (rendering concerns), mixed abstraction levels in JSX.

**The move:** Split a large component into focused children. Each child handles one visual section or one interaction.

**Steps:**
1. Identify distinct visual sections in the JSX (header, sidebar, list, form, etc.)
2. For each section, identify what props it needs from the parent
3. Create a new component file for each section
4. Move the JSX and its directly-related handlers into the new component
5. Pass data as props from the parent
6. Verify: the rendered output is visually identical

**How to verify safely:**
- Take a screenshot (or describe the current render) before extracting
- After extraction, verify the same visual output
- Check that interactions (clicks, hovers, form submissions) still work

**Incremental approach:** Start with the most self-contained section — the one that needs the fewest props from the parent. Leaf components are easiest to extract.

**Common mistakes:**
- Prop drilling — if extracting creates 5 levels of prop passing, you need context or state management, not more components
- Over-extraction — a component that renders 3 lines of JSX and takes 8 props is worse than inline JSX
- Extracting coupled sections — if two visual sections share state heavily, extract them together or extract the state into a hook first

---

## Introduce error boundary

**Addresses:** No error handling, fragile component trees.

**The move:** Wrap sections of the UI in error boundaries that catch rendering errors and display a fallback instead of crashing the entire page.

**Steps:**
1. Identify the component tree sections that can fail independently (a widget, a sidebar, a data visualization)
2. Create an error boundary component (React class component with `componentDidCatch`)
3. Wrap each independent section with the error boundary
4. Provide a meaningful fallback UI ("This section couldn't load. Retry?" — not a blank space)
5. Log the error to your error reporting service from `componentDidCatch`

**How to verify safely:**
- Temporarily throw an error in a child component and verify the boundary catches it
- Verify that sibling sections continue to render when one section errors
- Verify that the fallback UI is helpful, not a blank void

**Incremental approach:** Start with the highest-risk sections (data visualizations, third-party integrations, complex calculations). The page shell should always render.

**Common mistakes:**
- One giant error boundary around the entire app — this provides no granularity; a single widget failure takes down everything
- Error boundaries that show nothing — a blank space is worse than a crash because the user does not know something is wrong
- Not logging the error — the boundary should report to Sentry/DataDog/equivalent so you know about the crash

---

## Add types

**Addresses:** No types / `any` everywhere, unsafe refactoring.

**The move:** Incrementally add TypeScript types to the most-used interfaces, starting with the shapes that cross module boundaries.

**Steps:**
1. Identify the most-imported modules (these types are used everywhere)
2. Define the core domain types: `User`, `Order`, `Product` — whatever the domain is
3. Type the API response shapes (use the actual API responses as reference)
4. Replace `any` with specific types in the most-used functions
5. Let TypeScript errors guide you to the next place that needs typing

**How to verify safely:**
- Types do not change runtime behavior — adding types should never change what the code does
- Run `tsc --noEmit` to verify the types are consistent
- If adding types reveals a bug (type mismatch that was hidden by `any`), fix it as a separate change

**Incremental approach:** Type from the outside in. Start with API response types and shared interfaces. The internal implementation can stay loosely typed until you touch it for other reasons.

**Common mistakes:**
- Typing everything at once — this is a multi-week project for a large codebase. Do it gradually.
- Using complex generics where a simple union would suffice — AI code tends to be over-simple; do not make it over-complex
- Fighting the types with `as` casts — if you need a cast, the type is wrong. Fix the type, not the symptom.

---

## Centralize config

**Addresses:** Config scattered as magic numbers, hardcoded values.

**The move:** Pull all hardcoded values (URLs, timeouts, feature flags, limits) into a single config module that reads from environment variables.

**Steps:**
1. Grep for the known magic numbers and URL patterns (see DETECTION-PATTERNS.md)
2. Create a config module: `config.ts` (or `config/index.ts` for larger apps)
3. Define each config value with a name, type, and default: `export const API_TIMEOUT = Number(process.env.API_TIMEOUT) || 30000`
4. Replace hardcoded values with config references
5. Add validation: fail fast if required config is missing (API keys, database URLs)

**How to verify safely:**
- Each replacement is mechanical — same value, different source
- Add a startup check that validates all required config is present
- Test with the actual environment variables to verify the values resolve correctly

**Incremental approach:** Start with the values that appear in the most places. A URL used in 12 files provides more value when centralized than a timeout used once.

**Common mistakes:**
- Over-configuring — not everything needs to be configurable. A retry count of 3 that will never change can stay hardcoded
- Defaulting everything — some config (API keys, database URLs) should fail loudly if missing, not default to a dev value
- Config that is never read from environment — a config module that hardcodes values is just a renamed magic number file

---

## Create shared client

**Addresses:** Repeated API patterns, duplicated fetch logic.

**The move:** Replace repeated fetch/axios calls with a typed API client that handles auth, base URL, headers, error handling, and response parsing in one place.

**Steps:**
1. Identify the common patterns across API calls (headers, base URL, auth token, error handling)
2. Create an API client module: `api/client.ts`
3. Configure the base instance with shared settings (base URL, headers, timeout, interceptors)
4. Add typed methods for each endpoint: `client.getUser(id)`, `client.createOrder(data)`
5. Replace raw fetch/axios calls with client methods throughout the codebase
6. Add centralized error handling (transform HTTP errors into domain errors)

**How to verify safely:**
- Replace one API call at a time
- Verify the request (headers, body, URL) is identical before and after
- Verify error handling behavior is preserved (or improved)

**Incremental approach:** Build the client with the first replacement and add methods as you migrate each call. Do not try to map every endpoint upfront.

**Common mistakes:**
- An overly generic client that requires as much boilerplate as raw fetch — the client should make the 90% case trivial
- Swallowing errors in the client — transform and propagate them, do not hide them
- Coupling the client to a specific framework (React hooks inside the client) — keep it framework-agnostic so it can be used in services, scripts, and tests

---

## Kill dead code

**Addresses:** Dead code, noise reduction.

**The move:** Identify and remove unused code: functions, components, imports, commented-out blocks.

**Steps:**
1. Run dead code detection tools (`knip`, `ts-prune`, TypeScript strict mode, ESLint no-unused-vars)
2. For each finding, verify it is truly unused (some code may be used dynamically or in tests)
3. Delete it. Do not comment it out. It is in git history if you need it.
4. Remove any imports or dependencies that are now unreferenced

**How to verify safely:**
- Build passes after deletion (no compilation errors)
- Tests pass after deletion (nothing was depending on the "dead" code)
- If unsure, search for the function/component name across the entire codebase

**Incremental approach:** Start with the obvious: commented-out blocks, unused imports, files with zero importers. Move to the less obvious (functions exported but never imported) after the easy wins.

**Common mistakes:**
- Deleting code that is used dynamically (string-based imports, reflection, event handlers registered by name)
- Deleting test utilities or fixtures that appear unused in production but are used in tests
- Commenting out instead of deleting — this just creates more dead code. Delete it.

---

## Add boundary validation

**Addresses:** No validation at boundaries, unsafe inputs.

**The move:** Add input validation at every system boundary: API request handlers, form submissions, external service responses.

**Steps:**
1. Identify boundaries: API route handlers, form submit handlers, webhook receivers, external API response parsers
2. Choose a validation library: zod (TypeScript), yup (JavaScript), joi (Node.js), pydantic (Python)
3. Define schemas for each boundary: request body shapes, query parameter types, response shapes
4. Add validation at the boundary — reject invalid input before processing
5. Return meaningful error messages for validation failures

**How to verify safely:**
- Validation should not change behavior for valid inputs — existing valid requests still work
- Invalid inputs that previously caused downstream crashes now fail early with a clear error
- Test with both valid and invalid inputs at each boundary

**Incremental approach:** Start with the public API boundaries (user input). Then add validation to internal boundaries (service-to-service, API response parsing). Internal boundaries are lower priority but catch integration bugs.

**Common mistakes:**
- Validating too late — validation in the database layer is too late; the error message is useless. Validate at the boundary where you can return a helpful response
- Validating too strictly — breaking existing clients by rejecting inputs that previously worked (even if technically invalid). Add validation as "warn" before "reject" for existing APIs
- Not validating responses — an external API returning unexpected data should be caught, not passed through to crash your code

---

## General principles

### Order of operations

When a file has multiple smells, fix in this order:
1. **Add error handling first** — you need the safety net before you start moving code around
2. **Add types** — so the compiler catches mistakes during refactoring
3. **Extract services** — separate concerns so you can work on one layer at a time
4. **Extract hooks** — separate state from rendering
5. **Extract components** — clean up the rendering layer
6. **Kill dead code** — remove what is no longer needed after extraction
7. **Centralize config** — clean up the final cosmetic issues

### The refactoring test

After each move, ask: "Is this code easier for someone else to understand, modify, and debug?" If the answer is no — if you just moved complexity from one place to another — revert and try a different approach.

### When not to refactor

- **Code that will be deleted soon** — do not polish code on its way out
- **Code that works and never changes** — stable, untouched code is low-priority regardless of smell count
- **Code where the refactor costs more than rewriting** — sometimes starting over (for one module, not the whole app) is cheaper than incremental fixes
- **Code that has no tests and cannot be manually verified** — add tests first, then refactor
