# Retrospective

Review what happened after a deploy. Scale to the change size.

## When to use
After every feature or significant fix. Skip for trivial patches. Invoke with `/retrospective`.

## Quick retro (most changes)
```
1. First time? Y/N
2. Regression? Y/N
3. Lesson? [if any → 5 Whys below]
```

## Full retro (features or when something went wrong)

Use the **5 Whys** method on every issue found:

1. **What happened?** Describe the symptom.
2. **Why 1:** What was the immediate cause?
3. **Why 2:** Why did that cause exist?
4. **Why 3:** Why wasn't it caught earlier?
5. **Why 4:** What process gap allowed it?
6. **Why 5:** What principle, if followed, would prevent this class of problem?

Then:
- **Design right?** → No → Add question to `/plan`
- **Request clear?** → No → Add to Step 0
- **Automate?** → Yes → Create skill or hook

## Verification gate (MANDATORY)
After identifying the action:
- Which step/file was updated?
- Read it back. Does the update prevent this **class** of problem?
- If not, revise until it does.

A feedback loop without verification is a dead end.

## Actions
Update the relevant file:
- Root cause principle → CLAUDE.md Change Process (the step that should prevent it)
- Lessons Learned → CLAUDE.md table
- New check → `.claude/skills/review.md`
- New question → `.claude/skills/plan.md`
