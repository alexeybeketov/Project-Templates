# Audit

Comprehensive codebase audit for security, bugs, performance, and code quality.

## When to use
Before major releases or periodically. Invoke with `/audit`.

## Instructions

Launch 3 parallel audit agents for comprehensive coverage:

**Agent 1 — Backend:** server code, API, database, auth
```
Agent(subagent_type="Explore", model="opus", run_in_background=true,
  prompt="Audit backend for: security, auth, thread safety, data handling, error handling.")
```

**Agent 2 — Frontend:** UI code, HTML, CSS
```
Agent(subagent_type="Explore", model="opus", run_in_background=true,
  prompt="Audit frontend for: XSS, accessibility, performance, UX, mobile.")
```

**Agent 3 — Infrastructure:** config, deployment, dependencies
```
Agent(subagent_type="Explore", model="opus", run_in_background=true,
  prompt="Audit infra for: security, reliability, performance, container hardening.")
```

Compile results when all 3 complete. Use `/parallel-fix` to fix findings.

### Check areas per agent
- **Security:** auth bypasses, injection, credential exposure, input validation
- **Bugs:** race conditions, error handling, orphaned references, logic errors
- **Performance:** repeated operations, DOM rebuilds, missing caching
- **Code quality:** dead code, inconsistent patterns, inline styles

## Output format
```
## Audit — [date]

| # | Priority | File:Line | Issue | Fix |
|---|----------|-----------|-------|-----|
| 1 | Critical | ... | ... | ... |

Summary: N critical, N high, N medium, N low
```
