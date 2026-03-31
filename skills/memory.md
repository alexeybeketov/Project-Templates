# Memory

Manage persistent memory files across sessions.

## When to use
When learning something that should persist. Invoke with `/memory` to review, or save automatically when worth remembering.

## Memory types
| Type | Save when | Example |
|---|---|---|
| **user** | Learn about user's role, preferences | "User prefers prod-only workflow" |
| **feedback** | User corrects or confirms approach | "Don't use dev mode" |
| **project** | Learn about ongoing work, goals | "Merge freeze Thursday" |
| **reference** | Discover external resources | "Bugs tracked in Linear" |

## When to save
- User explicitly says "remember this"
- User corrects behavior
- User confirms a non-obvious approach
- Discovery of external resources
- Convert relative dates to absolute

## When NOT to save
- Code patterns derivable from reading code
- Git history / debugging solutions
- Anything in CLAUDE.md
- Ephemeral task details

## Session handoff pattern
When context is getting large or a session is ending with unfinished work:
1. Save current state to a project memory: what was being done, what's left, key decisions made
2. Next session can read the memory to resume without re-investigating
3. Delete the handoff memory once work is complete

## Verify before acting
Memory can be stale:
- File path → check exists
- Function name → grep for it
- Trust current state over remembered state

## Output format
```
## Memory Review

| File | Type | Status | Action |
|------|------|--------|--------|
| x.md | feedback | Current | Keep |
| y.md | project | Stale | Remove |
```
