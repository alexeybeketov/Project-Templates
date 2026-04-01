# Extract Component

Safely extract a React component from a monolith file.

## When to use
When splitting large single-file React apps. Invoke with `/extract-component`.

## Pre-flight checks (MANDATORY)
1. **Is it rendered?** `grep "<ComponentName"` — if 0, it's dead code
2. **Does it receive props?** Prop-based = safe. Closure = needs Context first
3. **Browser-test after EVERY extraction** — build success ≠ render success

## Types
- **Type A (prop-based):** Receives all data via props. Extract directly.
- **Type B (closure-bound):** Accesses parent state. Create Context first.
- **Type C (mixed):** Props for caller data, Context for shared state.

## Safe deletion for large files
1. Edit tool: replace first unique lines with comment marker
2. sed: delete between marker and next component
3. If fails: `git show HEAD:file` to recover content
4. NEVER use sed line ranges — use content markers

## Phased approach (strangler fig)
1. Inventory all inner components — categorize as A/B/C
2. Extract Type A first (prop-based modals) — lowest risk
3. Create domain Contexts for shared state
4. Extract Type B using useContext
5. Old+new coexist during migration
