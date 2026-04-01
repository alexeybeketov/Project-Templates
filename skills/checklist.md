# Checklist

Quick pre-commit gate that combines review + version + docs in one pass.

## When to use
Before every `git commit`. Invoke with `/checklist`. Faster than running `/review` + `/document` separately for small changes.

## Instructions

Run through each section. Output the table. All must PASS or be explicitly N/A.

### Code
- [ ] Does exactly what was asked — nothing more
- [ ] No `err.message` sent to clients
- [ ] No hardcoded secrets or credentials
- [ ] No `innerHTML` without sanitization
- [ ] SQL queries parameterized (no string interpolation of user input)
- [ ] CSRF middleware applied (not just defined)

### Version & Docs
- [ ] Version bumped if behavior changed
- [ ] CHANGELOG.md updated
- [ ] Affected docs updated (README, in-app help, CLAUDE.md)
- [ ] Doc headers match current version

### Git hygiene
- [ ] No secrets/backups/generated files staged (`git diff --cached --stat`)
- [ ] New generated directories in .gitignore
- [ ] Specific files staged (not `git add -A`)

## Output format
```
## Checklist — [description]

| Area | Result |
|------|--------|
| Code | PASS/FAIL |
| Version & Docs | PASS/FAIL |
| Git hygiene | PASS/FAIL |

Result: PASS / FAIL
```
