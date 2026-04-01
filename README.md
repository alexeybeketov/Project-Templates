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

### Skills (23)

| Category | Skills |
|---|---|
| **Per-change** | `/plan`, `/review`, `/checklist`, `/release`, `/verify`, `/retrospective`, `/document` |
| **Fast path** | `/quick-fix` |
| **Quality** | `/audit`, `/audit-staged`, `/test`, `/cleanup` |
| **Debug** | `/investigate`, `/hotfix` |
| **Strategy** | `/research`, `/architecture` |
| **Agents** | `/parallel-fix`, `/explore` |
| **Refactoring** | `/extract-component` |
| **System** | `/improve`, `/memory`, `/skill-builder`, `/sync-templates` |

### Hooks (settings.local.json)
- **Stop** — session exit checklist (document, retrospective, sync-templates, improve)

Note: PreToolUse hooks for blocking commits/edits were tested and found unreliable (`$TOOL_INPUT` variable content unpredictable). Enforcement is better handled via CLAUDE.md rules which are always in the conversation context.

## Template Sync Loop

Templates and project skills form a closed learning loop:

```
Templates ──import──> Project Skills ──improve──> Project Skills ──export──> Templates
                                                                                │
                                                                           git push
                                                                                │
Machine A ──push──> GitHub ──pull──> Machine B ──improve──> push ──> GitHub
    ▲                                                                    │
    └──────────────────────── pull ◄─────────────────────────────────────┘
```

**Rules:**
- Never blindly import all templates — investigate first (`/sync-templates`)
- Export universal improvements back to templates
- Keep project-specific customizations local
- Lessons that appear in 2+ projects become template lessons
- `/improve` includes template sync as step 6

## Multi-Machine Sync

Templates are synced to GitHub via `/sync-templates`. The skill includes phases for pulling, exporting, committing, and pushing changes via a secure wrapper script.

### Setup per machine
1. Clone this repo: `git clone <your-repo-url> ~/project-templates`
2. Set `TEMPLATE_DIR` to the clone path (e.g., `export TEMPLATE_DIR="$HOME/project-templates"`)
3. Create `~/.github-token` with a GitHub PAT (repo scope): `echo 'ghp_...' > ~/.github-token && chmod 600 ~/.github-token`
4. Copy `~/.git-push-secure.sh` from an existing machine (or create per the pattern in `/sync-templates`) and `chmod 700` it

**Security:** GitHub token is stored outside the repo in `~/.github-token` (permissions 600). Never committed, never logged.

## Philosophy

- Skills are documentation that becomes habit through use
- Every failure feeds back into the process (retrospective)
- Small improvements compound over time
- Enforce through CLAUDE.md rules (always in context), not hooks (unreliable)
- Templates learn from every project, every project learns from templates

## Adoption Guide

Expect friction during the first 3 sessions. The workflow feels slow at first because it adds steps before and after coding. Here's what to expect:

| Session | What happens | What to do |
|---------|-------------|------------|
| 1-2 | You'll skip `/verify`, `/document`, `/sync-templates` after deploying | User reminds. Add the step. It becomes visible. |
| 3-4 | Pre-coding steps (`/plan`, `/review`) feel natural. Post-deploy steps still get skipped. | Focus on Step 8 (Close) — force output from each sub-skill. |
| 5+ | Full workflow runs without reminders. New lessons flow back to templates. | The process is working. |

**Key insight from SPT adoption:** The post-deploy skills (`/verify`, `/document`, `/cleanup`, `/memory`, `/sync-templates`) break down because they feel like "extra work after the real work is done." The fix is making them explicit numbered steps with required output — not optional afterthoughts.

**Enforcement:** CLAUDE.md rules are more reliable than hooks. `$TOOL_INPUT` in hooks is unreliable. Put mandatory rules in CLAUDE.md where they're always in the conversation context.
