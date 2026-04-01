# Audit Staged

Git-diff aware security audit with exhaustive pattern scanning. Replaces `/audit` for all security audits.

## When to use
For all security audits. Invoke with `/audit-staged`.

## Architecture

```
Tier 1: Pattern Scan (every audit, all files, zero agent tokens)
Tier 2: Staged Agent Audit (changed files only since last baseline)
Tier 3: Full Agent Audit (quarterly, all files, multi-session)
```

## Instructions

### Phase 1: Pattern Scan (ALWAYS run — all files, instant)

Run grep-based scans for ALL known vulnerability patterns. Customize paths for your project.

**Security patterns to scan:**
1. `err.message` sent to client (not just logged)
2. SQL string interpolation (`${var}` inside queries, `INTERVAL '${var}'`)
3. `innerHTML` without sanitization
4. Hardcoded passwords/secrets in code (fallback defaults like `|| 'password'`)
5. `dangerouslySetInnerHTML`, `eval()`, `new Function()`, `document.write()`
6. Tokens in localStorage
7. Dev-mode auth bypass (`NODE_ENV !== 'production'` + fake user)
8. Content-Disposition headers with unsanitized filenames
9. `child_process` exec with variable interpolation
10. Open redirects (`window.location = userInput`)
11. Unescaped user data in HTML email templates
12. CSV export without formula injection protection
13. Missing CSRF on route mounts
14. `RETURNING *` (over-exposure, defense-in-depth)
15. Weak password fallback defaults in config files
16. Missing authorization on write endpoints

### Phase 2: Scope Check

```bash
# Get last audit baseline from memory
BASELINE=$(cat /tmp/.project_audit_baseline 2>/dev/null || git log --reverse --format="%H" | head -1)

# Find changed files since baseline
git diff --name-only $BASELINE HEAD -- '*.js' '*.jsx' '*.ts' '*.tsx' | grep -v node_modules
```

- **0 changed files** → Pattern scan only
- **1-15 changed files** → 1-3 agents on changed files
- **15+ changed files** → Split into batches, save progress to `/memory`

### Phase 3: Agent Audit (only on changed files)

- Use haiku for files < 200 lines
- Use opus for files > 200 lines or security-critical
- Check: auth, input validation, error handling, race conditions, business logic

### Phase 4: Update Baseline

```bash
echo "$(git rev-parse HEAD)" > /tmp/.project_audit_baseline
```

## Token optimization

| Method | Savings |
|--------|---------|
| Grep instead of agents for known patterns | 95% |
| Git diff for scope | 80% — only audit changed files |
| Haiku for simple files, Opus for complex | 60% cost |
| Session batching for Tier 3 | Fits within limits |

## Output format
```
## Audit Staged — [date]

### Tier 1: Pattern Scan (all files)
| # | Pattern | Matches | Status |
|---|---------|---------|--------|

### Tier 2: Changed Files
- Changed since baseline: N
- Audited: N

### Findings
| # | Priority | File:Line | Issue | Fix |
|---|----------|-----------|-------|-----|

### Baseline: [commit]
```
