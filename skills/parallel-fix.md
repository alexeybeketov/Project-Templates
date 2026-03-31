# Parallel Fix

Spawn multiple agents to fix independent issues simultaneously.

## When to use
When you have 2+ independent fixes on different files. Invoke with `/parallel-fix`.

## Modes

### Standard: split by file
Group issues by file dependencies. Launch one agent per group.

### Dual-agent comparison (for complex changes)
Spawn TWO agents with the same task but different instructions:
- Agent A: "Implement the minimal correct fix"
- Agent B: "Implement the cleanest, most maintainable solution"
Compare results, pick the better one or merge strengths.

## Instructions

### 1. Group by file dependencies
Two items are independent if they don't modify the same files.

### 2. Spawn in one message
Each agent prompt must be self-contained. Always say "Read the file first."

### 3. Verify and commit
Check `git diff --stat`, run pre-commit checks, commit as one atomic change.

## Rules
- Never two agents on the same file
- Check for shared utilities multiple files use
- Verify results before committing
- **Max 3 agents** per batch to keep results manageable
