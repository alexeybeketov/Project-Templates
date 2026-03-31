# Test

Run comprehensive functional tests.

## When to use
After changes, before releases. Invoke with `/test`.

## Instructions

### Test suites
<!-- Customize for your project -->
1. **Auth** — login, signup, logout, permissions
2. **CRUD** — create, read, update, delete for main entities
3. **API** — all endpoints return expected responses
4. **Security** — unauthorized access blocked, bad input rejected
5. **Edge cases** — empty states, large data, concurrent access

### Cleanup
- Delete test data created during testing
- Restore original state

## Output format
```
## Test Results

| Suite | Tests | Passed | Failed |
|-------|-------|--------|--------|
| Auth | N | N | N |
| **Total** | **N** | **N** | **N** |

Failures: [list any]
```
