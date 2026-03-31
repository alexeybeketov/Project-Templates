# Review

Multi-perspective code review before committing.

## When to use
After implementing, before committing. Invoke with `/review`.

## Perspectives (review from each angle)

### Security
- [ ] No auth bypasses on new endpoints
- [ ] User input validated at boundaries
- [ ] Error messages don't leak internals (return generic messages, log details server-side)
- [ ] No hardcoded secrets, IPs, credentials
- [ ] All user-controlled strings escaped before output (XSS)
- [ ] SQL queries use parameterized statements (no string interpolation)
- [ ] File writes use atomic pattern (temp + rename)
- [ ] CSRF middleware defined AND applied to route mounts (not just defined)

### Functionality
- [ ] Does exactly what was asked — nothing more
- [ ] All dependent files checked — search ALL views/instances (not just the first one found)
- [ ] If removing a function: grep for all references
- [ ] No data deletion to fix bugs — migrate in place

### UI State (if frontend)
- [ ] Async buttons have inProgress guards
- [ ] Intervals/timeouts tracked and cleared on close/navigate
- [ ] Modal close/escape blocked during operations
- [ ] All UI elements reset on retry/back/close
- [ ] React hooks at component top level (never inside IIFEs, loops, or conditionals)
- [ ] localStorage persistence effects don't fire on mount with empty state (use mount guard)

### Performance
- [ ] No repeated expensive operations per request
- [ ] No full DOM rebuilds for small changes
- [ ] Lazy loading for heavy resources

### Maintainability
- [ ] No debug logging in production
- [ ] Consistent styling patterns
- [ ] No dead code introduced
- [ ] Config/schema changes backward compatible

## Bounded fix loop
If review finds issues: fix → re-review. **Max 3 iterations.** If still failing after 3, escalate to the user — the approach may be wrong.

## Output format
```
## Review — [change description]

### Security: PASS/FAIL
### Functionality: PASS/FAIL
### UI State: PASS/FAIL (if applicable)
### Performance: PASS/FAIL
### Maintainability: PASS/FAIL

Result: PASS / FAIL (iteration N/3)
```
