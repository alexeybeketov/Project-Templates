# Architecture

Evaluate architectural decisions against core principles.

## When to use
When making structural decisions. Invoke with `/architecture [decision]`.

## Core principles

1. **Simplicity over abstraction** — three similar lines > premature abstraction
2. **Security by default** — validate at boundaries, auth on every endpoint
3. **Data preservation** — never delete data to fix bugs, migrate in place
4. **Progressive disclosure** — basic features visible, advanced collapsed

## Evaluation
1. Does it add complexity for the common case?
2. Can it be removed later without breaking existing users?
3. Does it create a new attack surface?
4. Would a new user understand this?

## Anti-patterns
- Building adapter interfaces for one implementation
- Adding config options instead of making good defaults
- Creating placeholder files for unbuilt features
- Exposing internal infrastructure in the UI

## Output format
```
## Architecture: [decision]

### Against principles
1. Simplicity: [impact]
2. Security: [impact]
3. Data: [impact]

### Recommendation
[Decision with reasoning]
```
