# Plan

Run Steps 0-2: Clarify, Investigate, Design — before writing any code.

## When to use
At the start of any feature or fix. Invoke with `/plan`. For trivial fixes, use `/quick-fix`.

## Quick scope check (ALWAYS do this)
1. **What elements does this change touch?** (functions, endpoints, UI components, timers)
2. **What happens on close/navigate/escape?** (cleanup timers, reset state, restore UI)
3. **What happens on double-click/rapid fire?** (guards, disabled states)
4. **What other code references what I'm changing?** (grep for function/variable names)
5. **Will this create new files/directories?** (add to .gitignore BEFORE creating if generated/sensitive)
6. **Does this change config for a third-party system?** (verify options exist in official docs before adding)
7. **Refactoring a component?** (grep for render references — is it actually used? Check closure deps. Browser-test every change)

## Full plan (for non-trivial changes)

### Step 0: Clarify
- What exactly needs to change?
- Why is it needed?
- Where in the code?
- What stays the same?

### Step 1: Investigate
- Read the relevant code — don't guess
- Check what files consume what you're changing
- If it's a bug, reproduce it

### Step 2: Design
- List 2+ approaches with tradeoffs
- Get user alignment before coding

## Output format
```
## Plan: [one-line description]

### Scope check
- Elements: [list]
- Close/escape: [what happens]
- Double-click: [guarded?]
- References: [what else uses this]

### Design
| | Option A | Option B |
|---|---|---|

**Files affected:** [list]
**Risk level:** low/medium/high
```
