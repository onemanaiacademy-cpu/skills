---
name: clean-code
description: >
  Use this skill whenever the user shares or pastes code files and wants them reviewed, cleaned, audited, or checked for quality issues. Trigger on phrases like "check my code", "review this file", "clean this up", "audit my code", "is this safe to publish", "is this production ready", or whenever the user uploads or shares code (any language) and implies they want feedback. Also trigger proactively when the user generates code using an AI tool and says something like "does this look good" or "can I ship this". This skill performs a comprehensive multi-dimensional code audit covering: AI-injected icon/emoji artifacts, code injection and phishing patterns, typos in variable names and strings, infinite loop risks, server-side API security (DDOS/CORS), frontend API call safety (self-DDOS), database/credential/HTTPS hygiene, API call volume limits per page (max 10–30 calls), lazy loader enforcement when API limits are applied, and page load time analysis for API-driven pages.
---

# Clean Code Skill

Perform a thorough, structured audit of user-submitted code. Check every dimension below. Always produce a clear, actionable report grouped by category. Be specific: quote the offending line(s) and explain why it's a problem and how to fix it.

---

## Step 1 — Identify the Code Context

Before auditing, determine:
- **Language / framework** (JS, Python, PHP, SQL, React, etc.)
- **Layer** (frontend, backend/server, database layer, or mixed)
- **Purpose** (API server, UI component, DB connector, utility script, etc.)

This determines which checks are most critical. All checks run regardless, but severity weighting differs by context.

---

## Step 2 — Run All Checks

### 🎨 CHECK 1: Icon & Emoji Injection (AI Artifact Pollution)

AI code generators sometimes inject decorative emoji/icons into code comments, string literals, variable names, or even logic. These cause encoding issues, break linters, and look unprofessional in production.

**Look for:**
- Emoji characters in any non-UI code (✅ ❌ 🔥 💡 ⚡ 🚀 etc.)
- Unicode decorative symbols in variable names, comments, or log strings
- Icon font class names hardcoded where they don't belong (e.g. `"fa-icon-rocket"` in backend logic)
- Non-ASCII characters that are clearly decorative rather than functional

**Report:** List every occurrence with line reference. Flag as `WARNING` if in comments/logs, `ERROR` if in logic or variable names.

---

### 🛡️ CHECK 2: Code Injection & Phishing Patterns

Look for dangerous patterns that could expose the app to injection attacks or be used maliciously.

**SQL Injection:**
- String-concatenated queries: `"SELECT * FROM users WHERE id = " + userId`
- Unparameterized inputs fed directly into queries
- Missing prepared statements / parameterized queries

**JS/Code Injection:**
- Use of `eval()`, `new Function()`, `setTimeout(string)`, `setInterval(string)`
- `innerHTML` assignments from user-controlled data without sanitization
- `document.write()` with dynamic content

**Phishing / Suspicious Patterns:**
- Hardcoded external URLs that redirect or load remote scripts dynamically
- `<script src="http://...">` loading from unrecognized third-party domains
- Base64-encoded strings being decoded and executed at runtime
- Obfuscated variable names mixed with network calls (potential data exfiltration)
- Any `fetch()` or `XMLHttpRequest` posting to an unexpected external domain

**Report severity:** `CRITICAL` for active injection vectors, `HIGH` for suspicious patterns requiring review.

---

### 🔤 CHECK 3: Typos, Spelling & Unclosed Tags

**Variable & function name typos:**
- Names that appear to be misspelled versions of common words (e.g. `userNaem`, `responese`, `dataBasse`)
- Inconsistent naming conventions that suggest a typo (camelCase broken mid-name)
- Single-letter variables in non-loop contexts where a descriptive name was likely intended

**String literal spelling:**
- User-facing strings with obvious spelling errors
- Error messages, labels, or log strings with typos

**HTML/Template unclosed tags:**
- `<div>`, `<span>`, `<form>`, `<input>` without corresponding closing tags
- Self-closing tags missing the `/` where required
- Mismatched opening/closing tag pairs

**Comments:**
- Obvious spelling errors in code comments (these reflect poorly in open source / team code)

**Report:** List each finding with suggested correction.

---

### ♾️ CHECK 4: Loop Safety & Infinite Loop Risk

**While / do-while loops:**
- Loops where the exit condition variable is never modified inside the loop body
- `while(true)` or `while(1)` without a guaranteed `break` path
- Exit conditions that depend on external state that may never change

**For loops:**
- Iterator that is mutated inside the loop in a way that could prevent termination
- Off-by-one errors that could cause unintended extra iterations or skipped iterations

**Recursion:**
- Recursive functions without a clear, reachable base case
- Recursion depth that could hit stack limits on realistic inputs
- Mutual recursion (A calls B calls A) with no guaranteed termination

**Async / event loops:**
- `setInterval` calls with no corresponding `clearInterval`
- Event listeners registered inside loops (causes N listeners per iteration)
- Promises that never resolve or reject (hanging async chains)

**Report severity:** `CRITICAL` for confirmed infinite loops, `HIGH` for patterns that are risky under certain inputs.

---

### 🌐 CHECK 5: Server-Side API Security (Backend / Node / Python / PHP etc.)

Only applies when code is server-side (Express, FastAPI, Laravel, etc.)

**DDOS Protection:**
- Are rate limiters applied to public endpoints? (e.g. `express-rate-limit`, `slowapi`, etc.)
- Is there any throttling or request queue on expensive operations?
- Are there no limits on payload size? (missing `body-parser` limits, no `MAX_CONTENT_LENGTH`)
- Are computationally expensive endpoints (file upload, image processing, DB-heavy) protected?

**CORS / Cross-Origin:**
- Is `Access-Control-Allow-Origin: *` used without restriction on sensitive endpoints?
- Are CORS headers configured to allow only known trusted origins?
- Is the OPTIONS preflight handled correctly?
- Are credentials (`withCredentials`) allowed from wildcard origins? (This is a security error)

**Authentication on API routes:**
- Are any sensitive routes exposed without authentication middleware?

**Report:** `CRITICAL` for wildcard CORS on credentialed routes, `HIGH` for missing rate limiting on public endpoints.

---

### 📡 CHECK 6: Frontend API Call Safety (No Self-DDOS)

Only applies when code makes API calls from the frontend (React, Vue, plain JS, etc.)

**Unthrottled calls:**
- API calls triggered directly in `onChange`, `onKeyUp`, or `onInput` without debounce or throttle
- Polling with `setInterval` calling an API at very short intervals (< 5 seconds) without back-off
- API calls inside `useEffect` with dependencies that change on every render (infinite loop pattern)

**Retry storms:**
- Retry logic without exponential back-off
- Retries that don't have a maximum attempt count

**Spamming patterns:**
- Multiple identical API calls triggered simultaneously (e.g. no deduplication / request cancellation)
- Missing `AbortController` to cancel in-flight requests when the component unmounts or re-renders

**Report:** `HIGH` for patterns that could hammer an API, `WARNING` for patterns that are wasteful but not immediately harmful.

---

### 🔐 CHECK 7: Database, Credentials & HTTPS Hygiene

**Credentials in frontend code:**
- Hardcoded API keys, passwords, secrets, tokens in any frontend file
- `.env` variables exposed via `REACT_APP_` or `VITE_` that should NOT be public
- Database connection strings in client-accessible code

**HTTP vs HTTPS:**
- Any `http://` URL used for API calls, form submissions, or resource loading in production code
- `fetch("http://...")` or `axios.get("http://...")` pointing to non-localhost addresses
- Form `action` attributes using HTTP

**Parameter safety:**
- URL parameters used to pass sensitive data (passwords, tokens, PII in query strings)
- GET requests used where POST + HTTPS should be used for sensitive data
- Credentials passed in request headers without TLS enforcement

**Database connectors:**
- Connection credentials not loaded from environment variables
- No connection pooling limits (could exhaust DB connections)
- Missing input sanitization before DB queries (also covered in Check 2, but reinforce here)

**Report:** `CRITICAL` for credentials in frontend or HTTP for sensitive calls, `HIGH` for missing HTTPS enforcement.

---

### 🔢 CHECK 8: API Call Volume per Page

Applies to any frontend page or component that fetches data from APIs.

**Limit:** A single page should make no more than **10–30 API calls** on load or within a single user interaction. Exceeding this is a strong signal of architectural problems.

**What to count:**
- Every `fetch()`, `axios.get/post/put/delete()`, `useQuery()`, `useSWR()`, `$.ajax()`, or similar call triggered on page mount or interaction
- Count parallel calls (e.g. `Promise.all([...])` with N calls counts as N)
- Count calls inside loops that fire per-item without batching

**What to flag:**
- More than 30 API calls on a single page load → `CRITICAL`
- 15–30 calls with no batching strategy → `HIGH`
- 10–15 calls with no pagination or virtualization for list data → `WARNING`

**Recommended fixes:**
- Batch multiple resource fetches into a single API endpoint
- Use GraphQL or BFF (Backend for Frontend) to aggregate data
- Paginate or virtualize lists instead of fetching all items at once
- Cache responses and reuse across components (React Query, SWR, Redux RTK Query)

**Report:** Count the total estimated API calls, flag severity, and suggest batching or caching strategies.

---

### 🔄 CHECK 9: Lazy Loader Enforcement for API-Limited Pages

Applies when a page makes more than 10 API calls **or** fetches lists of items from an API.

**Rule:** Whenever API call limits are applied or large datasets are fetched, a lazy loading strategy MUST be present to manage data inflow and prevent UI blocking.

**What to look for (missing patterns):**
- List data fetched all at once with no pagination, infinite scroll, or virtual list
- Images or media loaded eagerly without `loading="lazy"` or a lazy-load library
- Components that fetch their data on mount simultaneously instead of deferring off-screen content
- No skeleton screens, placeholder states, or progressive rendering while data loads
- `Promise.all()` fetching everything in parallel with no priority ordering (critical data first, secondary data deferred)

**Accepted lazy load patterns (look for these — flag if absent):**
- **Pagination:** `page`, `offset`, `cursor`-based API params
- **Infinite scroll:** IntersectionObserver triggering next-page fetch
- **Virtual lists:** `react-window`, `react-virtual`, `@tanstack/virtual`
- **Image lazy loading:** `loading="lazy"` on `<img>`, or libraries like `react-lazyload`
- **Deferred component fetch:** `React.lazy()` + `Suspense`, dynamic `import()`
- **Priority loading:** Critical above-the-fold data first, secondary data after

**Report severity:** `HIGH` if API calls exceed 10 with no lazy loading present. `WARNING` if lazy loading exists but is incomplete (e.g. images eager, lists lazy).

---

### ⏱️ CHECK 10: Page Load Time & API-Driven Performance

Applies to any page that populates content via API calls.

**Goal:** Identify code patterns that will cause slow or blocked page loads when API data is involved.

**Blocking patterns (flag these):**
- Data fetching done synchronously before render (no loading state shown to user)
- Sequential `await` chains where calls could run in parallel: `await fetchA(); await fetchB();` → should be `Promise.all([fetchA(), fetchB()])`
- No loading/skeleton state — page appears blank until all API calls complete
- Large JSON payloads fetched in full when only a subset is needed (over-fetching)
- No response caching — identical API calls repeated on every render or navigation
- Waterfalling: component fetches data only after parent finishes fetching, creating a request chain
- No timeout or deadline set on API calls (a slow endpoint blocks render indefinitely)

**Performance targets to check against:**
- API calls should ideally resolve within **2–3 seconds** for perceived responsiveness
- Above-the-fold content should not depend on more than **3–5 API calls**
- List endpoints should return **paginated** results (e.g. max 20–50 items per page)

**What good looks like (check for these positive patterns):**
- Skeleton/shimmer UI shown while data loads
- `Promise.all()` or concurrent fetching for independent data sources
- Cache-Control headers respected or client-side cache (SWR `staleWhileRevalidate`, React Query `staleTime`)
- Timeouts set via `AbortController` with a deadline (e.g. 10s max)
- Above-the-fold data prioritized; below-the-fold deferred

**Report:** List any blocking or waterfall patterns found. Flag missing skeleton/loading states. Suggest parallelization or caching where applicable. Severity: `HIGH` for confirmed blocking render patterns, `WARNING` for missing optimizations.

---

## Step 3 — Produce the Audit Report

Structure the output as follows:

```
## 🔍 Clean Code Audit Report

**File / Context:** [filename or description]
**Language:** [detected language]
**Layer:** [frontend / backend / database / mixed]

---

### Summary
| Category | Status | Issues Found |
|---|---|---|
| 🎨 Icon Injection | ✅ Clean / ⚠️ Warning / 🚨 Error | N |
| 🛡️ Injection & Phishing | ... | N |
| 🔤 Typos & Unclosed Tags | ... | N |
| ♾️ Loop Safety | ... | N |
| 🌐 Server API Security | ... / N/A | N |
| 📡 Frontend API Safety | ... / N/A | N |
| 🔐 Credentials & HTTPS | ... | N |
| 🔢 API Call Volume | ... / N/A | N |
| 🔄 Lazy Loader Enforcement | ... / N/A | N |
| ⏱️ Page Load Performance | ... / N/A | N |

---

### [Category Name] — [Status]

**Issue:** [description]
**Line / Location:** [quote the problematic code]
**Risk:** [what could go wrong]
**Fix:** [concrete recommended fix with code example if helpful]

[repeat per issue]

---

### ✅ Overall Verdict
[READY TO PUBLISH / NEEDS MINOR FIXES / NOT SAFE TO PUBLISH]

[1–2 sentence summary of the most important things to address before shipping.]
```

---

## Step 4 — Tone & Delivery Guidelines

- Be direct but constructive. The user wants to ship good code, not be lectured.
- Always provide a fix, not just the problem.
- Prioritize by severity: `CRITICAL` → `HIGH` → `WARNING` → `INFO`.
- If a category has no issues, say "✅ Clean — no issues found." Don't skip it.
- If a category is N/A for the code's context (e.g. no server code present), say "N/A — not applicable for this code type."
- Keep the overall verdict simple and actionable.
