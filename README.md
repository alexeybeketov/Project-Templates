# Project Templates for Claude Code

Reusable templates for CLAUDE.md, skills, and hooks. Battle-tested across 50+ releases.

## Quick Setup

```bash
# Copy to your project
cp CLAUDE.md /your/project/CLAUDE.md
mkdir -p /your/project/.claude/skills
cp skills/*.md /your/project/.claude/skills/
cp settings.local.json.template /your/project/.claude/settings.local.json
```

Then customize `CLAUDE.md` for your project (architecture, endpoints, build commands).

## What's included

### CLAUDE.md
8-step change process with gates:
0. Clarify → 1. Investigate → 2. Design → 3. Implement → 4. Review → 5. Release → 6. Done → 7. Document → 8. Retrospective

### Skills (22)

| Category | Skills |
|---|---|
| **Per-change** | `/plan`, `/review`, `/release`, `/verify`, `/retrospective`, `/document` |
| **Fast path** | `/quick-fix` |
| **Quality** | `/audit`, `/test`, `/cleanup` |
| **Debug** | `/investigate`, `/hotfix` |
| **Strategy** | `/research`, `/architecture` |
| **Agents** | `/parallel-fix`, `/explore` |
| **System** | `/improve`, `/memory`, `/skill-builder`, `/sync-templates` |

### Hooks (settings.local.json)
- **PreToolUse** on `git commit` — blocks commit if `/plan` and `/review` weren't run
- **PostToolUse** on skill edits — reminds to run `/sync-templates`
- **PostToolUse** on `git tag` — reminds to run `/document` and `/retrospective`
- **Stop** — reminds about `/retrospective` at session end

## Template Sync Loop

Templates and project skills form a closed learning loop:

```
Templates ──import──> Project Skills ──improve──> Project Skills ──export──> Templates
                                                                       │
                                                                  git push
                                                                       │
                                                                    GitHub
                                                                       │
                                                                  git pull
                                                                       │
                                                                Other Machines
```

**Rules:**
- Never blindly import all templates — investigate first (`/sync-templates`)
- Export universal improvements back to templates
- Keep project-specific customizations local
- Lessons that appear in 2+ projects become template lessons
- `/improve` includes template sync as step 6

## Multi-Machine Sync

Templates are synced to GitHub at [Project-Templates](https://github.com/alexeybeketov/Project-Templates). The `/sync-templates` skill includes a Phase 4 that commits and pushes changes via a secure wrapper script.

**Security:** GitHub token is stored outside the repo in `~/.github-token` (permissions 600). Never committed, never logged.

## Philosophy

- Skills are documentation that becomes habit through hooks
- Every failure feeds back into the process (retrospective)
- Small improvements compound over time
- Automate enforcement, not judgment
- Templates learn from every project, every project learns from templates
