# Audit

Comprehensive codebase audit for security, bugs, performance, and code quality.

## When to use
Before major releases or periodically. Invoke with `/audit`.

## Instructions

Use parallel Explore agents for thorough analysis:
- Agent 1: Backend code (security, auth, data handling)
- Agent 2: Frontend code (XSS, UX, performance)
- Agent 3: Infrastructure (config, deployment, dependencies)

### Security checks
- Auth on all endpoints
- User data escaped before output
- Input validation at boundaries
- No hardcoded credentials
- No path traversal vulnerabilities

### Bug checks
- Orphaned function references
- Race conditions in concurrent code
- Error handling gaps

### Performance checks
- Repeated expensive operations
- DOM rebuilds on large datasets
- Missing caching

### Code quality
- Dead code
- Inconsistent patterns
- Inline styles vs CSS classes

## Output format
```
## Audit — [date]

| # | Priority | File:Line | Issue | Fix |
|---|----------|-----------|-------|-----|
| 1 | Critical | ... | ... | ... |

Summary: N critical, N high, N medium, N low
```
