# DETECTION-PATTERNS.md

How to quickly find smells without reading every file. Specific commands and patterns to run, what the output means, and how to triage results. Use these during the scan step (Step 2) to efficiently surface candidates for deeper review.

---

## Find god components

**Goal:** Identify files that are doing too much — the 300+ line components that are likely god components.

**Commands:**

```bash
# Sort source files by line count (top 20 largest)
find src -name "*.tsx" -o -name "*.jsx" -o -name "*.vue" -o -name "*.svelte" | xargs wc -l | sort -rn | head -20

# Count useState calls per file (more than 5 is suspicious)
grep -rcl "useState" src --include="*.tsx" --include="*.jsx" | xargs -I {} sh -c 'echo "$(grep -c "useState" {}) {}"' | sort -rn | head -10

# Count useEffect calls per file (more than 3 is suspicious)
grep -rcl "useEffect" src --include="*.tsx" --include="*.jsx" | xargs -I {} sh -c 'echo "$(grep -c "useEffect" {}) {}"' | sort -rn | head -10

# Find files with many imports (more than 15 imports = too many concerns)
grep -rcl "^import" src --include="*.ts" --include="*.tsx" | xargs -I {} sh -c 'echo "$(grep -c "^import" {}) {}"' | sort -rn | head -10
```

**What the output means:**
- Files over 300 lines: review for god component patterns
- Files with 5+ useState: likely managing too much state
- Files with 3+ useEffect: likely doing too many side effects
- Files with 15+ imports: likely mixing concerns from many domains

**Triage:** Open the top 5 largest files and scan for mixed concerns. If a single file has fetch calls, state management, business logic, and rendering, it is a god component regardless of line count.

---

## Find duplication

**Goal:** Identify copy-paste code and near-duplicate components.

**Commands:**

```bash
# Find files with similar names (likely copy-paste variants)
find src -name "*.tsx" -o -name "*.jsx" | xargs basename -a | sort | uniq -d

# Count how many files make raw fetch/axios calls (should be 0-3 if there is a client)
grep -rl "fetch(" src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | wc -l
grep -rl "axios\." src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | wc -l

# Find repeated string patterns (URLs, constants used in multiple files)
grep -rn "http://\|https://" src --include="*.ts" --include="*.tsx" | grep -v "node_modules" | sort

# Find files that import the same module many times (indicates duplication of use)
grep -rh "from ['\"]" src --include="*.ts" --include="*.tsx" | sort | uniq -c | sort -rn | head -20
```

**What the output means:**
- Duplicate basenames: likely copy-paste variants (UserCard + TeamCard, Modal + Dialog)
- 10+ files with raw fetch/axios: no shared API client exists
- Same URL in multiple files: config not centralized
- Same import in 10+ files: that module is a core dependency; check if each usage follows the same pattern

**Triage:** Files with similar names are the highest-value targets. Open them side by side and check for structural similarity.

---

## Find missing error handling

**Goal:** Identify code that assumes success — fetch without catch, async without try, no error boundaries.

**Commands:**

```bash
# Find await statements not inside try blocks (heuristic: await on a line where the previous 5 lines contain no "try")
grep -rn "await " src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | head -30

# Find .then() without .catch()
grep -rn "\.then(" src --include="*.ts" --include="*.tsx" | grep -v "\.catch("

# Find fetch calls without error handling nearby
grep -rn "fetch(" src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx"

# Check for error boundary components
grep -rl "componentDidCatch\|ErrorBoundary" src --include="*.tsx" --include="*.jsx"

# Check for global error handler
grep -rn "window.onerror\|window.addEventListener.*error\|process.on.*uncaughtException" src
```

**What the output means:**
- `await` without try/catch: potential unhandled rejection
- `.then()` without `.catch()`: promise chain that swallows errors
- fetch calls: cross-reference with error handling nearby (is there try/catch or .catch?)
- No error boundary files: rendering errors will crash the entire app
- No global error handler: uncaught errors will be invisible

**Triage:** Focus on await/fetch calls in the critical paths first (authentication, payment, data mutation). Missing error handling in a utility function is lower priority than in a checkout flow.

---

## Find type gaps

**Goal:** Identify places where TypeScript provides no safety — `any`, type assertions, and suppressions.

**Commands:**

```bash
# Count explicit any usage
grep -rn ": any\|: any\[\]\|as any" src --include="*.ts" --include="*.tsx" | wc -l

# Find ts-ignore and ts-expect-error (compiler warnings suppressed)
grep -rn "@ts-ignore\|@ts-expect-error" src --include="*.ts" --include="*.tsx"

# Find untyped function parameters (functions with no type annotations)
grep -rn "function.*(.*).*{" src --include="*.ts" --include="*.tsx" | grep -v ":"

# Find Record<string, any> patterns (typed but meaningless)
grep -rn "Record<string, any>" src --include="*.ts" --include="*.tsx"

# Check if API responses are typed
grep -rn "\.json()\|\.data" src --include="*.ts" --include="*.tsx" | grep -v ":" | head -20
```

**What the output means:**
- High `any` count relative to codebase size: types provide no real safety
- `@ts-ignore`: the developer gave up on a type error instead of fixing it
- Untyped function params: callers get no autocomplete or validation
- `Record<string, any>`: looks typed but is equivalent to untyped
- Untyped API responses: the data shape is unknown after network calls

**Triage:** Focus on shared interfaces first — types that are used across multiple modules provide the most value when correctly typed. A single internal `any` is low priority; an API response type used in 15 components is high priority.

---

## Find dead code

**Goal:** Identify code that is unused — never imported, never called, commented out but not removed.

**Commands:**

```bash
# Find large commented-out blocks (3+ consecutive comment lines)
grep -rn "^[[:space:]]*//" src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | awk -F: '{print $1}' | uniq -c | sort -rn | head -10

# Find exports that may be unused (check against import usage)
grep -rn "export function\|export const\|export class" src --include="*.ts" --include="*.tsx" | awk -F: '{print $3}' | head -20

# Check for unused dependencies in package.json
# (Use knip or depcheck for comprehensive analysis)
npx knip --include dependencies 2>/dev/null || npx depcheck 2>/dev/null

# Find files with zero importers (orphaned files)
# Use knip or ts-prune for comprehensive analysis
npx knip --include files 2>/dev/null || npx ts-prune 2>/dev/null
```

**What the output means:**
- Files with many comment lines: likely contain commented-out code blocks
- Exports not imported anywhere: potentially dead code (verify dynamic usage)
- Unused dependencies: packages in package.json never imported in source
- Orphaned files: files that exist but are never imported by any other file

**Triage:** Commented-out code blocks are safe to delete immediately (they are in git history). Unused exports need more verification — they may be used dynamically or in tests. Unused packages are safe to remove from package.json.

---

## Find config scatter

**Goal:** Identify hardcoded values that should be centralized — URLs, ports, timeouts, API keys.

**Commands:**

```bash
# Find hardcoded URLs
grep -rn "http://localhost\|http://127\.0\.0\.1\|https://api\.\|https://.*\.com" src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | grep -v "node_modules\|test\|spec\|__test__"

# Find hardcoded port numbers
grep -rn ":[0-9]\{4\}\b\|port.*[0-9]\{4\}" src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | grep -v "node_modules"

# Find hardcoded timeouts (common values: 5000, 10000, 30000, 60000)
grep -rn "5000\|10000\|30000\|60000" src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | grep -v "node_modules\|test"

# Find potential API keys (strings that look like keys)
grep -rn "sk-\|pk_\|AKIA\|ghp_\|xoxb-" src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" --include="*.env*"

# Check if a config/env module exists
find src -name "config.*" -o -name "env.*" -o -name "constants.*" | head -5
```

**What the output means:**
- Hardcoded URLs in source files: will break when environments change
- Port numbers scattered: makes port changes require multi-file edits
- Timeout values in source: cannot tune without code changes
- API key patterns in source: secrets that should be in environment variables
- No config module: config is definitely scattered

**Triage:** API keys in source are always critical (security issue). URLs in multiple files are high priority (operational issue). Timeouts in one place are low priority (cosmetic).

---

## Find TODO debt

**Goal:** Identify load-bearing TODOs that represent missing functionality vs. aspirational TODOs that represent nice-to-haves.

**Commands:**

```bash
# Find and count all TODOs/FIXMEs
grep -rn "TODO\|FIXME\|HACK\|XXX" src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | wc -l

# Categorize by type (look for keywords that indicate structural debt)
grep -rn "TODO.*error\|TODO.*auth\|TODO.*valid\|TODO.*secur\|TODO.*handl" src --include="*.ts" --include="*.tsx"
grep -rn "TODO.*config\|TODO.*env\|TODO.*hard.?cod" src --include="*.ts" --include="*.tsx"
grep -rn "HACK\|XXX\|FIXME" src --include="*.ts" --include="*.tsx"

# List all TODOs with context
grep -rn -B1 -A1 "TODO\|FIXME" src --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx"
```

**What the output means:**
- TODOs mentioning error/auth/validation/security: structural debt that affects safety
- TODOs mentioning config/env/hardcoded: operational debt that affects deployability
- HACK/XXX/FIXME: the developer knew this was wrong when they wrote it
- High total count (20+): nobody is managing the TODO list — they accumulate without resolution

**Triage:** Structural TODOs (auth, errors, validation) are high priority. Operational TODOs (config, env) are medium. Feature TODOs ("TODO: add dark mode") are low — they belong in an issue tracker, not in code.

---

## Find dependency bloat

**Goal:** Identify unnecessary, duplicated, or oversized dependencies.

**Commands:**

```bash
# Count total dependencies
cat package.json | python3 -c "import sys,json; d=json.load(sys.stdin); print('dependencies:', len(d.get('dependencies',{}))); print('devDependencies:', len(d.get('devDependencies',{})))"

# Check bundle size of largest dependencies (if bundlephobia is available)
# Otherwise, check node_modules size
du -sh node_modules 2>/dev/null
ls node_modules | wc -l

# Find duplicate functionality (multiple packages for same purpose)
grep -l "moment\|dayjs\|date-fns\|luxon" package.json  # date libraries
grep -l "axios\|node-fetch\|got\|ky" package.json      # HTTP clients
grep -l "lodash\|ramda\|underscore" package.json        # utility libraries

# Check for packages with no imports in source
for pkg in $(cat package.json | python3 -c "import sys,json; [print(k) for k in json.load(sys.stdin).get('dependencies',{}).keys()]"); do
  count=$(grep -rl "\"$pkg\"\|'$pkg'\|from ['\"]$pkg" src 2>/dev/null | wc -l)
  if [ "$count" -eq "0" ]; then echo "UNUSED: $pkg"; fi
done
```

**What the output means:**
- 50+ runtime dependencies for a simple app: bloated
- Multiple packages for the same purpose (2 date libraries, 2 HTTP clients): inconsistency and bloat
- Packages with no imports in source: unused dependencies that add install time and attack surface
- Very large node_modules relative to app complexity: worth investigating what is pulling in so many transitive deps

**Triage:** Unused dependencies are easy wins — remove them. Duplicate-purpose packages indicate inconsistency (pick one and migrate). Oversized dependencies (moment.js when you only format one date) are medium priority — replace when you touch that code.

---

## Quick assessment checklist

Run these first to get a rapid picture of codebase health:

```bash
# How big is the codebase?
find src -name "*.ts" -o -name "*.tsx" -o -name "*.js" -o -name "*.jsx" | xargs wc -l | tail -1

# How many files?
find src -name "*.ts" -o -name "*.tsx" -o -name "*.js" -o -name "*.jsx" | wc -l

# Largest files (god component candidates)?
find src -name "*.tsx" -o -name "*.jsx" | xargs wc -l | sort -rn | head -5

# Any tests?
find . -name "*.test.*" -o -name "*.spec.*" -o -name "__tests__" | wc -l

# Any types (or all any)?
grep -rc ": any" src --include="*.ts" --include="*.tsx" 2>/dev/null | awk -F: '{sum+=$2} END {print sum " any usages"}'

# TODO count?
grep -rc "TODO\|FIXME" src --include="*.ts" --include="*.tsx" --include="*.js" 2>/dev/null | awk -F: '{sum+=$2} END {print sum " TODOs"}'

# Error handling?
grep -rc "try\|\.catch(" src --include="*.ts" --include="*.tsx" 2>/dev/null | awk -F: '{sum+=$2} END {print sum " error handling points"}'
```

This gives you a 60-second snapshot: codebase size, complexity concentration (largest files), safety (tests, types, error handling), and debt (TODOs). Use it to decide where to focus the deeper scan.
