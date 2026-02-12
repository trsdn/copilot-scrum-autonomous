---
name: sprint-start
description: "Start sprint execution: review state, set up, execute issues with quality gates and daily huddles. Triggers on: 'sprint start', 'start sprint', 'let's begin', 'go', 'start working', 'begin sprint'."
---

# Sprint Start

You are the **Scrum Master**. Begin sprint execution.

**Stakeholder model**: Per `docs/constitution/PROCESS.md`, sprints start automatically.
Only escalate if MUST criteria are met. No consent gate needed.

## Step 1: Gather State

```bash
git log --oneline -10
git status
git stash list
gh issue list --label "priority:high"
gh issue list --label "status:in-progress"
```

Read agent memory for context from last sprint.

## Step 2: Load Sprint Backlog

If `/sprint-planning` was run, use the Planned items from the board.
Otherwise, present prioritized candidates and select based on ICE scoring.

Determine the sprint number from `docs/sprints/velocity.md` (increment from last sprint).
Create SQL todos for all sprint items with dependencies.

## Step 3: Create Sprint Log

Create a sprint log file to document huddle results and decisions during execution:

```bash
mkdir -p docs/sprints
cat > docs/sprints/sprint-N-log.md << 'EOF'
# Sprint N Log — [Date]

**Goal**: [sprint goal]
**Planned**: [number] issues

## Huddles

[Huddles will be appended here after each issue completes]
EOF
```

## Step 4: Start Execution

Check `docs/constitution/PROCESS.md` MUST escalation criteria:
- If any sprint issue triggers MUST criteria → notify stakeholder, wait for that issue only
- Otherwise → **begin execution immediately** (no consent gate)

Send summary notification:
```bash
if [ -n "$NTFY_TOPIC" ]; then
  curl -s -H "Title: 🚀 Sprint N Starting" -d "Goal: [theme]. Issues: #A, #B, #C..." ntfy.sh/$NTFY_TOPIC
fi
```

## Step 5: Execute (issue by issue)

For each issue in the sprint backlog:

### 5a. Start Issue
Move to "In Progress" on the board. Create worktree:
```bash
git worktree add -b <branch-name> ../<project>-<short-id> main
cd ../<project>-<short-id>
```

### 5b. Implementation Flow
```
implement → lint/type-check → write unit tests → validate → code review → PR → merge
```

### 5c. Quality Gates

**⛔ TEST GATE**: Every feature PR MUST include unit tests.
- Use `test-engineer` agent after implementation, before PR
- Minimum 3 tests per feature (happy path, edge case, parameter effect)
- Tests must verify **actual behavior**, not just "runs without error"

**⛔ DEFINITION OF DONE** (see CLAUDE.md for full checklist):
- Code + lint + types clean
- Unit tests (min 3, behavior-verifying)
- PR reviewed + squash-merged
- CI green before merge
- Issue closed with summary
- **Board updated**: Move issue to "Done" on project board

### 5d. 🔄 Daily Huddle (after each issue completes)

**Do NOT skip this.**

**Document the huddle in two places:**

**1. Comment on the completed issue** (persistent record):
```bash
gh issue comment N --body "### Huddle — Sprint X, Issue X/Y done

**Outcome**: [what was delivered, key metric or result]
**Key learning**: [anything discovered during implementation]
**Decision**: [any re-prioritization or scope change]
**Next**: #M — [title]"
```

**2. Append to sprint log** (`docs/sprints/sprint-N-log.md`):
```markdown
### Huddle — After Issue #N ([timestamp])

**Completed**: #N — [outcome]
**Sprint progress**: X/Y issues done
**Key learning**: [anything that changes the plan?]

**Plan check**:
- Does anything we learned change the priority of remaining issues?
- Should we re-order?
- Any blockers?

**Next up**: #M — [title]
```

**Why document**: Huddle decisions and learnings are lost when the session ends. The sprint log preserves context for retros, and issue comments create a traceable audit trail.

## Constraints

- **Sub-agents**: ≤2 concurrent (avoid PTY exhaustion)
- **WIP**: 1 issue at a time — finish before starting next
- **Sprint focus**: Never silently abandon an in-progress issue

## ⛔ Agent Dispatch Rules

**NEVER use the generic `task` agent type (Haiku) for code changes.**
Haiku lacks the reasoning capacity for multi-file edits and often describes changes without executing them.

Use the correct custom agent for each task:

| Task | Agent Type | Model |
|------|-----------|-------|
| Code implementation | `code-developer` | Sonnet |
| Writing tests | `test-engineer` | Sonnet |
| Code review | `code-review` | Sonnet |
| Research/docs | `research-agent` or `documentation-agent` | Sonnet |
| Quick file search | `explore` | Haiku (OK) |
| Build/test commands | `task` | Haiku (OK) |

**Rule**: `task` (Haiku) is ONLY for running commands (build, test, lint) where you need pass/fail output. All creative/coding work MUST use a custom agent or `general-purpose` (Sonnet).

**⛔ CI GATE**: After creating a PR, wait 3-5 minutes for CI to complete. Verify green with `gh run list --branch <branch> --limit 3` BEFORE merging. Never merge without CI green.

## Output Format

```markdown
## Sprint — [Date]

### Sprint Backlog (ordered)
1. #N: [Title] (priority, orchestration)
2. #M: [Title] (priority, orchestration)

### Progress
- [X] #N — Done (PR #P)
- [ ] #M — Next
```

When all issues are done or time is exhausted → use `/sprint-review` and `/sprint-retro`.
