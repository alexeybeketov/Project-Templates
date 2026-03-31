# Explore

Deep codebase exploration for a specific question.

## When to use
When understanding how something works across multiple files. Invoke with `/explore [question]`.

## Context loading limit
Load **max 5-8 files** per investigation to prevent context overflow. If more files are needed, split into multiple explorations.

## Instructions

Use an Explore agent to avoid cluttering main context:
```
Agent(subagent_type="Explore", prompt="[question]", description="Explore: [3 words]")
```

### Good use cases
- "How does auth flow work end-to-end?"
- "What files consume this config?"
- "Find all places user input reaches the output"
- "Trace the data flow from input to storage"

### When NOT to use (use direct tools instead)
- Simple file lookup → Glob
- Single pattern search → Grep
- Known file path → Read

### Thoroughness
Specify in prompt: "quick", "medium", or "very thorough"

### Fallback (if Agent unavailable)
Use Grep + Read directly

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
