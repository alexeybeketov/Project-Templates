# Skill Builder

Review the project and create/update skills to automate repetitive workflows.

## When to use
Periodically or when noticing repeated patterns. Invoke with `/skill-builder`.

## Instructions

### 1. Gather context
- Read CLAUDE.md for the change process
- Check recent git history for patterns
- List existing skills

### 2. Identify automatable workflows
Look for patterns that are:
- Repeated across sessions
- Multi-step with consistent sequence
- Error-prone without automation

### 3. Create or update skills
Write skill files in `.claude/skills/` with:
- Clear name and description
- When to use
- Step-by-step instructions
- Output format

### 4. Validate against failure cases
Before deploying a new skill:
- Test against the **easy case** (should succeed)
- Test against the **hard case** (should succeed OR fail gracefully)
- A skill validated only on easy cases will fail silently on hard cases

### 5. Report
- Skills created/updated
- Workflows that can't be automated (and why)
- Validation results (easy + hard case)
