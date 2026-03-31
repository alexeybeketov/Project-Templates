# Sync Templates

Bidirectional sync between project skills and the universal template library.

## When to use
- Before importing templates into a new/existing project
- After improving a skill in any project
- Periodically during `/improve`
Invoke with `/sync-templates`.

## The loop

```
Templates ──import──> Project Skills ──improve──> Project Skills ──export──> Templates
    │                                                                           │
    └────────────── learning flows both ways ───────────────────────────────────┘
```

## Instructions

### Phase 1: Investigate before importing
Before copying templates into a project, DO NOT blindly import. First:

1. **Read existing project skills** (if any)
   ```bash
   ls .claude/skills/ 2>/dev/null
   ```

2. **Compare template vs project version** for each skill
   - Template has something project doesn't? → Candidate to import
   - Project has something template doesn't? → Candidate to export back
   - Both have different approaches? → Evaluate which is better

3. **Check project context**
   - Does the project have a different tech stack? (templates assume Docker/git/CI)
   - Does the project have different workflows? (monorepo vs single repo)
   - Does the project have domain-specific needs? (different security model, different deploy)

4. **Selective import** — never copy all templates blindly
   - Import skills that fill gaps
   - Skip skills that conflict with existing project patterns
   - Adapt skills that need project-specific customization

### Phase 2: Export improvements back to templates
After improving a skill in any project:

1. **Is this improvement universal?** (applies to all projects, not project-specific)
   - Yes → update the template
   - No → keep it project-local only

2. **Is the template version outdated?**
   - Compare template timestamp vs project skill timestamp
   - If project is newer AND the change is universal → update template

3. **Update the template**
   ```bash
   # Strip project-specific details, keep universal patterns
   cp .claude/skills/improved-skill.md /volume1/home/nas_alexey/project-templates/skills/
   # Review: remove project-specific commands, paths, endpoints
   ```

### Phase 3: Cross-project learning
When working on multiple projects:

1. **Collect lessons** — each project's CLAUDE.md Lessons Learned table has unique insights
2. **Find universal patterns** — if the same lesson appears in 2+ projects, it belongs in the template
3. **Update template CLAUDE.md** — add universal lessons to the template's Lessons Learned table
4. **Propagate** — on next `/sync-templates` in other projects, the lesson flows there

## Decision matrix

| Situation | Action |
|---|---|
| Template has skill, project doesn't | Import if relevant to project |
| Project has skill, template doesn't | Export to template if universal |
| Both have same skill, template newer | Compare — import if template is better |
| Both have same skill, project newer | Compare — export if improvement is universal |
| Skill is project-specific | Keep project-only, don't export |
| Lesson appears in 2+ projects | Add to template CLAUDE.md |

## What NOT to sync
- Project-specific commands (deploy scripts, API URLs)
- Hardcoded paths, IPs, credentials
- Project-specific checklist items (specific endpoint names)
- Domain-specific patterns (camera presets, FTP config)

## Phase 4: Push to GitHub

After exporting improvements to templates, commit and push to GitHub so other machines can pull:

1. **Check for changes**
   ```bash
   git -C /volume1/home/nas_alexey/project-templates status --short
   ```

2. **Stage changed files by name** (never `git add -A`)
   ```bash
   git -C /volume1/home/nas_alexey/project-templates add <specific files>
   ```

3. **Commit with context**
   ```bash
   git -C /volume1/home/nas_alexey/project-templates commit -m "sync: <what changed>"
   ```

4. **Push using the secure wrapper** (token stored in ~/.github-token with 600 permissions)
   ```bash
   /volume1/home/nas_alexey/.git-push-secure.sh /volume1/home/nas_alexey/project-templates
   ```

### Security rules
- **NEVER** hardcode tokens in skill files, CLAUDE.md, or any git-tracked file
- **NEVER** put tokens in git remote URLs
- **NEVER** log or echo the token value
- Token is stored ONLY in `~/.github-token` (permissions 600, not git-tracked)
- The push wrapper script is OUTSIDE the repo (not git-tracked)
- If the token needs rotation: update `~/.github-token` only

## Output format
```
## Template Sync — [date]

### Imported (template → project)
- [skill] — [what was added]

### Exported (project → template)
- [skill] — [what improvement]

### Skipped
- [skill] — [why: project-specific / not applicable]

### Lessons synced
- [lesson] — [from which project]
```
