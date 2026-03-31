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
<!-- Add lessons as they occur -->
