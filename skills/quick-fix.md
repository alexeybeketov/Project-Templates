# Quick Fix

Fast path for small, obvious changes.

## When to use
For changes < 20 lines, no security/data/API impact. Invoke with `/quick-fix`.

## Criteria (all must be true)
- Change is < 20 lines
- No new endpoints or API changes
- No security implications
- No data format changes

## Instructions
1. **Fix** — make the change
2. **Review** — quick self-check: no XSS, no orphaned refs, no hardcoded values
3. **Release** — run `/release`
