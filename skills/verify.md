# Verify

Run deployment verification on the running system.

## When to use
After a release or restart. Invoke with `/verify`.

## Core checks (always run)
1. **Health** — hit the health endpoint, confirm new version
2. **Processes** — verify all expected services are running
3. **Logs** — check recent logs for errors/tracebacks
4. **Feature** — verify the specific thing that changed works

## Conditional checks
- **UI changed** → hard-refresh browser, check visually
- **Data changed** → verify existing data preserved
- **Mobile affected** → test on mobile browser
- **Config changed** → verify service restarts cleanly

## Docker-specific checks
- **ContainerConfig error** → `docker rm -f <container>` then redeploy
- **Multi-network routing** → verify `traefik.docker.network` label set for services on 2+ networks
- **Stale container name** → `docker ps -a --filter "name=<service>"` to find actual name/ID

## Output format
```
## Verify — vX.Y.Z

| Check | Result |
|-------|--------|
| Health | PASS — vX.Y.Z |
| Processes | PASS |
| Logs | PASS — clean |
| Feature | PASS |
```
