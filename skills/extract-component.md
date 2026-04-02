# Extract Component

Safely extract a React component from a monolith file.

## When to use
When splitting large single-file React apps. Invoke with `/extract-component`.

## Pre-flight checks (MANDATORY)
1. **Is it rendered?** `grep "<ComponentName"` — if 0, it's dead code
2. **Does it receive props?** Prop-based = safe. Closure = needs Context first
3. **Exhaustive dep check:** Extract ALL identifiers from body, filter React/CSS/HTML builtins. Remainder = closure deps. Do NOT just grep for known variables.
4. **Import path depth:** Match `../` depth to file location relative to context/utils
5. **Browser-test after EVERY extraction** — build success ≠ render success

## Types

### Type A (prop-based) — LOW RISK
Receives all data via props. Extract in 3 phases:
```
Phase A1: Create files (agents — haiku model, parallel batches of 4)
  - Agent creates new file, does NOT modify parent
Phase A2: Wire imports (manual — single Edit per batch)
  - Add imports, build to verify
  - CRITICAL: Run `grep -oP '<[A-Z][A-Za-z]+' newfile | sort -u` to find ALL JSX component refs
  - Verify each ref has an import — build passes but undefined components render blank
Phase A3: Remove inline definitions (manual — sed bottom-up)
  - Find boundaries, remove from bottom to top, build + browser verify
```

### Type B (closure-bound) — HIGH RISK
Accesses parent state via closure. Create Context first, then extract.

### Type C (mixed)
Props for caller data, Context for shared state.

## Safe deletion for large files
1. Find boundary: `awk "NR > START && /^  const [A-Z]/ { print NR; exit }"`
2. Remove bottom-up (prevents line number shifts)
3. If fails: `git show HEAD:file` to recover content
4. NEVER use sed line ranges — use boundary detection

## Phased approach (strangler fig)
1. Inventory all inner components — categorize as A/B/C
2. Extract Type A first (prop-based) — lowest risk, haiku agents
3. Create domain Contexts for shared state
4. Extract Type B using useContext
5. Old+new coexist during migration
