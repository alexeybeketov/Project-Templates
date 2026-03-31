# Document

Update documentation after a change.

## When to use
After every feature or fix. Invoke with `/document`.

## Check each file
1. **README.md** — user-facing features, setup, security
2. **In-app help** — mirrors README
3. **CLAUDE.md** — architecture, endpoints, dev notes, lessons learned
4. **Memory files** — user preferences, project context

## What NOT to document
- Internal implementation details publicly (security risk)
- Specific iteration counts, session limits
- Personal data (IPs, paths, hardware models)
- Placeholder features not yet built

## Output format
```
## Documentation — vX.Y.Z

| File | Updated | What changed |
|------|---------|-------------|
| README | Yes/No | [what] |
| Help | Yes/No | [what] |
| CLAUDE.md | Yes/No | [what] |
```
