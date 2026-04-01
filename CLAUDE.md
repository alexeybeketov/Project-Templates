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
- Step 0-2: Use `/plan` skill
- Step 4: Use `/review` skill
- Step 5: Use `/release` skill (includes `/verify`)
- Step 7: Use `/document` skill
- Step 8: Use `/retrospective` skill
- Debugging: Use `/investigate` or `/hotfix`
- Before features: Use `/research` and `/architecture`
- Periodic: Use `/improve`, `/audit`, `/test`

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

### Step 6: Definition of Done
- [ ] Code implements what was agreed
- [ ] All review checks pass
- [ ] Deploy verified
- [ ] No regressions
- [ ] For UI: user confirmed it looks right

### Step 7: Document
Update if behavior changed:
- README.md — user-facing
- CLAUDE.md — architecture, endpoints, dev notes
- In-app help — mirrors user-facing docs

### Step 8: Retrospective (MANDATORY — never skip)
Use the **5 Whys** on every issue: symptom → cause → why it existed → why not caught → process gap → **principle**.

1. **Did it work first time?** → No → 5 Whys → Add principle to Lessons Learned
2. **Did it cause a regression?** → Yes → Add check to Step 4
3. **Was the design right?** → No → Add question to Step 2
4. **Was the request clear?** → No → Add to Step 0
5. **Can something be automated?** → Yes → Implement it

**Verify the loop is closed:** Which step was updated? Read it back. Does it prevent this class of problem? If not, revise.
- *Learn:* Are the right questions being asked at each step? Should steps be reordered, merged, split, or added?

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
<!-- Add lessons as they occur -->
