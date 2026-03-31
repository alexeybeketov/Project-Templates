# Investigate

Standalone debugging for production issues.

## When to use
When something is broken or behaving unexpectedly. Invoke with `/investigate [symptom]`.

## Context loading limit
Read **max 5-8 files** per investigation. If the issue spans more, split into focused sub-investigations.

## Instructions

### 1. Gather evidence (max 5 diagnostic commands)
- Health endpoint status
- Process status
- Recent logs (errors, tracebacks)
- Disk space / resource usage
- Database state

### 2. Apply 5-Why
1. **What** is broken? (specific, observable)
2. **Why 1:** Immediate cause
3. **Why 2:** Why did that cause exist?
4. **Why 3:** Why wasn't it caught earlier?
5. **Why 4:** What process gap allowed it?
6. **Why 5:** What principle prevents this class of problem?

Fix at the root level, not the symptom.

### 3. Bounded diagnosis
**Max 3 investigation rounds.** If root cause isn't found after 3 rounds, document what's known and escalate to the user with a hypothesis.

## Output format
```
## Investigation: [symptom]

- Evidence: [observed]
- 5-Why: [chain]
- Root cause: [problem] (confidence: high/medium/low)
- Fix: [recommendation]
```
