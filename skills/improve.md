# Continuous Improvement

Periodic review of the entire development system using 5-Whys root cause analysis.

## When to use
At session boundaries or after milestones. Invoke with `/improve`. Should also be triggered by `/retrospective` when patterns repeat.

## Instructions

### For each area, apply 5-Whys on any finding:

### 1. Process health
- Steps being skipped? → **Why?** → Why wasn't it caught? → Why no enforcement? → What principle prevents this class of skip?
- Steps too slow? → **Why?** → What's the bottleneck? → Can it be split/parallelized?
- Bugs past review? → **Why?** → What check was missing? → Why wasn't it in the checklist? → What category of check is underrepresented?
- Reworking designs? → **Why?** → What question would have surfaced the issue earlier?

### 2. Skill health
- Are skills being used? → **Why not?** → Too heavy? Not enforced? Not discoverable?
- Are skills accurate? → Does the skill match the actual workflow? If not, why did it drift?
- Any workflows still manual? → **Why?** → Can it be a skill, hook, or script?

### 3. Architecture health
- Complexity justified? → **Why** does this abstraction exist? → Is it serving 1 use case or many?
- Dead code? → **Why** was it created? → Why wasn't it cleaned up sooner?
- Performance issues? → **Why?** → Where's the bottleneck? → Is it architectural or implementation?

### 4. Security posture
- New endpoints without auth? → **Why** wasn't auth added? → Was there a checklist gap?
- Data exposure? → **Why?** → What review would have caught it?
- Credentials in code/config? → **Why?** → What's the rotation policy?

### 5. Velocity
- What took longest? → **5 Whys** on the slowest item
- What was most repeated? → Can it be automated? → **Why** isn't it already?
- What blocked progress? → **Why?** → Is it a tool gap, knowledge gap, or process gap?

### 6. Template sync
- Run `/sync-templates`
- Any project lessons that should be universal? → **Why** is this lesson project-specific vs universal?
- Any template improvements not yet imported?

## Verification gate
For each action taken:
- Which file was updated?
- Read it back. Does it prevent this **class** of problem?
- If not, revise until it does.

## Output format (MANDATORY — each finding MUST have full 5-Why chain)
```
## Improvement Review — [date]

### Findings
For EACH finding, output the FULL chain:

1. **What?** [observable problem]
   **Why 1:** [immediate cause]
   **Why 2:** [why that cause existed]
   **Why 3:** [why it wasn't caught]
   **Why 4:** [process/skill gap]
   **Root principle:** [what prevents this CLASS of problem]
   **Action:** [file updated + what changed]
   **Verified:** [re-read — does it prevent the class?]

### Template sync
- Exported: [list]

### Verification
- [file updated] → prevents [class of problem]? Yes/No
```
