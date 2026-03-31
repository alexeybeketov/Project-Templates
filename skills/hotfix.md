# Hotfix

Diagnose and recover from a broken deploy or production issue.

## When to use
When something is broken after deploy. Invoke with `/hotfix`.

## Instructions

### 1. Diagnose
- Check health endpoint
- Check logs for errors/tracebacks
- Check all expected processes running
- Check disk space and database state

### 2. Decide: rollback or fix-forward?

**Rollback** if: service won't start, critical security issue, no obvious fix
**Fix-forward** if: bug identified, fix is small, service is running

### 3. Fix loop (bounded)
Fix → verify → if still broken → fix again. **Max 3 iterations.** If still broken after 3 attempts, rollback and escalate.

### 4. After recovery
Run `/verify` then `/retrospective`

## Common issues
| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| Health returns old version | Deploy didn't complete | Re-trigger deploy |
| 502/503 errors | App process not started | Check logs |
| Config error on start | Bad config file | Validate syntax |
| Auth failures | Rate limited or expired | Restart service |
| Docker ContainerConfig KeyError | Stale container state after build | `docker rm -f <container>` then redeploy |
| White screen (React) | Hooks inside render IIFE/nested function | Move hooks to component top level |
| 504 Gateway Timeout (Traefik) | Multi-network container, wrong IP | Add `traefik.docker.network` label |
| Redis ECONNREFUSED | Password with `/` breaks URL parsing | Use hex-only passwords in Redis URLs |

## Output format
```
## Hotfix — [issue]

- Symptom: [observed]
- Root cause: [why]
- Fix: [what] (iteration N/3)
- Verification: PASS/FAIL
- Prevention: [what to add]
```
