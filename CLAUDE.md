# CLAUDE.md

## Project Overview
<!-- Describe your project: what it does, who it's for, how it runs -->

## Architecture
<!-- Components, how they connect, key technologies -->

## Key Files
<!-- Table of important files and their purpose -->
| File | Purpose |
|---|---|
| | |

## Build & Run
<!-- How to build, start, and develop -->

## API Endpoints
<!-- Table of endpoints if applicable -->
| Method | Path | Auth | Purpose |
|---|---|---|---|

## Data Paths
<!-- Where data is stored, config files, databases -->

## Security
<!-- Authentication, authorization, encryption, input validation -->

## Development Notes
<!-- Gotchas, conventions, testing approach -->

## Change Process

**MANDATORY: Follow these steps in order for EVERY change.**

**Self-improving process:** Every step includes a micro-learn that can improve ANY step — not just the current one. Ask high-level questions that trigger thinking. When a gap is found, generalize the fix into a principle, not a one-off rule.

**Rules:**
- Do NOT write code before completing Steps 0-2 (clarify, investigate, design)
- Do NOT skip the release — every feature/fix gets a version bump and deploy
- Do NOT move to the next task before running the retrospective (Step 8)
- Do NOT skip the review checklist (Step 4)
- Do NOT start a new task until Steps 6-7 are complete
- If any gate fails, go back to the indicated step
- If the request is unclear, ASK — do not guess (Step 0)
- Batch fixes (audit, multi-file) still follow the process — `/plan` once for scope, `/review` before each commit
- Config changes to third-party systems MUST verify options exist in official docs (Step 1: Investigate)

**Skills (USE THEM — they exist in .claude/skills/):**
- Step 0-2: `/plan`
- Step 4: `/review` (full) or `/checklist` (quick pre-commit gate)
- Step 5: `/release`
- Step 6: `/verify`
- Step 7: `/document`
- Step 8: `/retrospective`, `/cleanup`, `/memory`, `/improve`, `/sync-templates`
- Debugging: `/investigate` or `/hotfix`
- Before features: `/research` and `/architecture`
- Periodic: `/improve`, `/audit`, `/test`

**Visibility enforcement:**
- Before each step, output `**Step N: Name**` as a header
- Before committing, output the Step 4 checklist with each item marked
- Before announcing deploy complete, output verification results
- If any step is skipped, the missing header makes it visible

### Step 0: Clarify
- **What** exactly needs to change?
- **Why** is it needed?
- **Where** in the code?
- **How much** should change?
- **What stays the same?**
- **Gate:** Can you write a one-sentence description of exactly what you'll do?

### Step 1: Investigate
- Check Lessons Learned table
- Read the relevant code — don't guess
- Reproduce the issue if it's a bug
- Trace root cause using 5-Why
- **Gate:** Can you explain the root cause?

### Step 2: Design
- List at least 2 approaches with tradeoffs
- Evaluate: security, mobile, state, breaking changes, maintenance, data impact
- Present recommendation, get alignment before coding
- **Gate:** User has agreed to the specific approach

### Step 3: Implement & Version
- Write the minimal correct change
- Check dependency map: what other files consume what you're changing?
- If removing a function: grep for ALL references across ALL files
- Does every claim match reality? (If something is called "cached," is it? If logic exists in multiple places, are all copies consistent?)
- **Gate:** Implementation matches what was agreed
- *Learn:* Is the implementation consistent across the codebase? Are there patterns that should be shared?

### Step 4: Review
Self-review the diff:
- [ ] Does exactly what was asked — nothing more, nothing less
- [ ] No new security issues
- [ ] User input validated at boundaries
- [ ] Error messages don't leak internals
- [ ] No removed functions still referenced
- [ ] No hardcoded secrets, IPs, or credentials
- [ ] No data deletion — migrate in place
- [ ] If scaling computation: runtime validated against hardware constraints
- **Gate:** All checkboxes pass
- *Learn:* Is the security posture strong enough? Could this change break something untested?

### Step 5: Release, Deploy & Verify
**5a. Commit and push:**
- Stage files by name (never `git add -A`)
- Write commit message explaining **why**
- Push to origin/main

**5b. Release and deploy:**
- Bump VERSION, commit, push
- Create tag and push → triggers CI/CD
- Deploy and wait for new version
- **Gate:** Build succeeds, deploy verified

**5c. Verify:**
- [ ] Health check — confirm new version
- [ ] Processes running
- [ ] Logs clean
- [ ] Feature works end-to-end
- [ ] If UI changed: hard refresh and check visually

### Step 6: Verify
- [ ] Health check — confirm new version running
- [ ] All processes/containers running
- [ ] Logs clean — no errors
- [ ] Feature works end-to-end
- [ ] If UI changed: hard refresh and check visually
- **Gate:** Output the `/verify` table

### Step 7: Document
Update if behavior changed:
- CHANGELOG.md — version entry with what changed
- README.md — user-facing features
- CLAUDE.md — architecture, endpoints, dev notes, lessons learned
- In-app help — mirrors user-facing docs
- **Gate:** Output the `/document` table

### Step 8: Close — STOP. Do not start next task.
Post-deploy is a sequence, not one step. Run ALL or explicitly skip with reason:

**8a. `/retrospective`** (always):
Use the **5 Whys** on every issue: symptom → cause → why it existed → why not caught → process gap → **principle**.
1. **Did it work first time?** → No → 5 Whys → Add principle to Lessons Learned
2. **Did it cause a regression?** → Yes → Add check to Step 4
3. **Was the design right?** → No → Add question to Step 2
4. **Was the request clear?** → No → Add to Step 0
5. **Can something be automated?** → Yes → Implement it

**Verify the loop is closed:** Which step was updated? Read it back. Does it prevent this class of problem?

**8b. `/cleanup`** (always): Remove dangling images, old builds, check disk space.
**8c. `/memory`** (if session ending): Save handoff state for next session.
**8d. `/improve`** (if milestone): Review process, skills, architecture health.
**8e. `/sync-templates`** (if skills changed): Export universal improvements, check for leaks.

## Lessons Learned

| # | Pattern | Rule | Incident Count |
|---|---------|------|----------------|
| 1 | Clarify first | Restate the request before coding | |
| 2 | Solution design | Evaluate 2+ options before implementing | |
| 3 | Function removal | Grep for ALL references before committing | |
| 4 | UI state management | Async ops need: inProgress guard, interval cleanup, close/escape blocking, reset | |
| 5 | Docker cleanup | After builds: `docker image prune -f` and `docker builder prune --all -f` — old images don't auto-clean | Wizard + SV |
| 6 | Data preservation | NEVER delete/clear data to fix a bug. Migrate, transform, or add a column instead | Wizard + SV |
| 7 | Docker ContainerConfig | `docker rm -f <container>` before `docker-compose up -d --no-deps` prevents KeyError on recreate | SPT |
| 8 | React hooks placement | Never place useState/useRef/useEffect inside nested render functions (IIFEs) — always at component top level | SPT |
| 9 | localStorage mount guard | Reset useEffects fire on mount with empty state, overwriting saved values. Use useRef mount guard to skip first render | SPT |
| 10 | Multi-network Traefik | Services on 2+ Docker networks need `traefik.docker.network` label or Traefik picks wrong IP (504) | SPT |
| 11 | Redis URL passwords | Passwords containing `/` break ioredis URL parsing. Use hex-only passwords for Redis URLs | SPT |
| 12 | CSRF defined ≠ applied | Defining CSRF middleware but not adding it to route mounts = zero protection. Verify middleware is in the `app.use()` chain | SPT |
| 13 | Error message leakage | Never return `err.message` to clients — DB errors expose schema. Return generic message, log details server-side | SPT |
| 14 | Fix ALL instances | Two tables/views showing same data — fixing one misses the other. Always search for ALL instances before declaring complete | SPT |
| 15 | Verify config fields | Traefik crashed on invalid `maxAge`/`maxSize` fields. Always verify config options exist in target system's official docs before adding | SPT |
| 16 | Process under pressure | Skipping /plan and /review during batch fixes caused a deploy-breaking config error. Follow the process especially when it feels urgent | SPT |
| 17 | Content-Disposition injection | Unsanitized filenames in Content-Disposition headers allow header injection via quotes/CRLF. Always strip `"`, `\r`, `\n`, control chars from filenames | SPT |
| 18 | Error handler safe default | `NODE_ENV === 'production'` fails open (leaks on missing var). Use `NODE_ENV === 'development'` to fail closed — only leak in explicit dev mode | SPT |
| 19 | Rate limiting consistency | All backend services in a platform must have rate limiting. Missing it on one service creates an asymmetric DoS vector | SPT |
| 20 | Audit sampling gap | Agent-based audits sample 5-8 files per run. Always follow with exhaustive grep scans for known vulnerability patterns across ALL files | SPT |
| 21 | SQL column interpolation | `${appCode}_role` from user params enables SQL injection even when the value passes a DB lookup. Use a whitelist map instead | SPT |
| 22 | Pattern repetition vs shared modules | Repeated security patterns (CSRF, auth, error handler) across multiple backends = many findings per audit. Extract into shared modules — one fix applies everywhere. Automation catches pattern violations; human review catches novel issues | SPT |
| 23 | Post-deploy is a sequence | `/verify`, `/document`, `/cleanup`, `/memory` consistently skipped when treated as optional. Make them explicit steps in the Change Process — each must produce output or be explicitly skipped with reason | SPT |
| 24 | Hardcoded passwords survive file deletion | Deleting legacy files misses the same passwords duplicated in active files. Always grep for the actual credential string across the entire repo, not just the known file | SPT |
| 25 | Nginx `add_header` inheritance trap | Security headers in server block are replaced (not appended) by `add_header` in nested location blocks. Static asset locations lose all security headers unless they duplicate them | SPT |
| 26 | `.dockerignore` per build context | Root `.dockerignore` only applies to `context: .` builds. Sub-app builds (`context: ./jce`) need their own `.dockerignore` or `node_modules`/`.env` leak into build layers | SPT |
| 27 | Dev-mode auth bypass | `if (NODE_ENV !== 'production') { req.user = fakeAdmin }` grants Admin to all requests if env var is unset. Never fail open on auth — fail closed with 503 | SPT |
| 28 | HTML injection in emails | User-controlled data (username, name) interpolated into HTML email templates without escaping enables phishing content in admin emails. Always `escapeHtml()` user data in templates | SPT |
| 29 | Hardcoded fallback secrets | `ENCRYPTION_KEY="${VAR:-default}"` in scripts means backups encrypted with a public key when env var is unset. Fail closed — refuse to run without the secret | SPT |
| 30 | INTERVAL SQL interpolation | `INTERVAL '${days} days'` is injectable even with `parseInt` (NaN becomes literal string). Use `$1 * INTERVAL '1 day'` with parameterized integer | SPT |
| 31 | IDOR on every mutation endpoint | 34 HIGH endpoints found where any authenticated user could delete any record by ID. Every POST/PUT/DELETE needs role authorization, not just authentication | SPT |
| 32 | Magic byte validation | MIME type from Content-Type header is user-controlled. Validate actual file content (magic bytes) after upload, before DB insert. Delete file on mismatch | SPT |
| 33 | GitHub token scopes | Creating `.github/workflows/` files requires token with `workflow` scope. Verify before pushing or the push will be rejected | SPT |
| 34 | ZAP internal vs external | ZAP scanning internal nginx shows missing headers that Traefik adds externally. Always scan the external URL for accurate results | SPT |
| 35 | Dockerfile COPY for new directories | Adding a new source directory (schemas/, tests/) requires updating the Dockerfile COPY directives. Container crashes with MODULE_NOT_FOUND otherwise | SPT |
| 36 | Step 8 skipping is structural | Gentle reminders don't prevent Step 8 skipping. Use "STOP. Do not start next task." as hard break. If still skipped, the process needs automation not instruction | SPT |
| 37 | Partial code replacement leaves dangling blocks | When replacing inline validation with middleware, remove the ENTIRE old block including closing brace. Build and verify before commit | SPT |
| 38 | Zod .partial() for PUT schemas | POST schemas validate required fields. PUT schemas use `.partial()` — all fields optional since updates are incremental. Clean reuse pattern | SPT |
| 39 | Per-field import schemas | Import endpoints use different field names (watches, levels, types). Create one schema per field name rather than a generic one. Catches mismatched field names at Zod level | SPT |
| 40 | Agent batching for mechanical changes | Zod wiring across 104 endpoints done via parallel agents (2-3 per batch). Each agent handles one backend. Pattern: create schemas → wire to routes → build → verify | SPT |
| 41 | Component extraction breaks closure scope | Components defined inside a parent function share state via closure. Extracting to separate files breaks this. Need React Context or prop drilling BEFORE extraction. Build succeeds but white screen at runtime | SPT |
| 42 | Build success ≠ render success | Vite build passing and container healthy does NOT mean the React app renders. Frontend refactors MUST be browser-verified before commit | SPT |
| 43 | Verify render before refactor | Before extracting/refactoring a component, grep for `<ComponentName` to verify it's actually rendered. 3 components extracted that were dead code — wasted effort + caused bugs | SPT |
| 44 | Safe large-file extraction pattern | sed line ranges failed 3x on 47K file. Working pattern: (1) Edit tool to replace first unique lines with comment, (2) sed to remove body between markers, (3) `git show HEAD:file` to recover content for new file, (4) Edit tool for render updates. Always browser-verify | SPT |
| 45 | Grep-based dep analysis misses variables | Grepping for known variable names misses deps. Exhaustive approach: extract ALL identifiers from component body, filter React/CSS/HTML builtins, remainder = closure deps. Also: import paths differ by directory depth | SPT |
| 46 | Skills must be validated on failure cases | A skill validated only on easy cases fails silently on hard cases. /skill-builder now requires testing both easy AND hard cases before deploying | SPT |
| 47 | 5-Whys skipped when not in mandatory output format | /improve had 5-Whys as guidance but not as a gate. Consistently skipped for shallow finding→action. Output format now requires full chain per finding with verification | SPT |
| 48 | Research findings don't flow to execution skills | /research produced AST analysis recommendation that never reached /plan or /extract-component. /research output now requires "Skills to update" section. /plan checks research → skill translation | SPT |
| 49 | JSX component refs ≠ closure deps | AST dep analysis tracked closure variables but missed JSX component references (`<ModalName />`). After extraction, run `grep -oP '<[A-Z][A-Za-z]+' file | sort -u` to find ALL component refs and verify each has an import. Build passes but undefined components render blank at runtime | SPT |
<!-- Add lessons as they occur -->
