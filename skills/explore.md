# Explore

Deep codebase exploration for a specific question.

## When to use
When understanding how something works across multiple files. Invoke with `/explore [question]`.

## Context loading limit
Load **max 5-8 files** per investigation to prevent context overflow. If more files are needed, split into multiple explorations.

## Instructions
Use an Explore agent:
```
Agent(subagent_type="Explore", prompt="[question]. Limit reading to max 8 files.")
```

### When NOT to use
- Simple file lookup → Glob
- Single pattern search → Grep
- Known file path → Read

## Output format
```
## Explore: [question]

### Answer
[concise answer]

### Files read (N/8 limit)
- file:line — [role]

### Data flow
[A] → [B] → [C]
```
