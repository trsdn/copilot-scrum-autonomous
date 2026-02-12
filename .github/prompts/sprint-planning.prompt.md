---
name: Sprint Planning
description: Triage backlog, score issues, select sprint scope, move to Planned
---

# Sprint Planning

You are the **Scrum Master** running sprint planning autonomously.

**Stakeholder model**: Per `docs/constitution/PROCESS.md`, sprints plan and start automatically.
Only escalate if MUST criteria are met (ADR changes, strategic direction, new dependencies, etc.).

**No code changes during planning. Only issue management, research, and documentation.**

## Step 1: Review Board State

```bash
gh project item-list <PROJECT_NUMBER> --owner <OWNER> --limit 30
gh issue list --label "priority:high"
gh issue list --label "priority:medium"
gh issue list --limit 30
```

## Step 2: Triage Unlabeled Issues

Score using ICE framework:

| Dimension | High (3) | Medium (2) | Low (1) |
|-----------|----------|------------|---------|
| **Impact** | Core functionality or architecture | Improves robustness or tooling | Nice-to-have |
| **Confidence** | Strong evidence or clear requirements | Some evidence | Speculative |
| **Effort** | Config-only, < 1 sprint | Minor code + testing | Multi-sprint |

Score = Impact × Confidence / Effort. ≥4 = high, 2-3 = medium, <2 = low.

```bash
gh issue edit N --add-label "priority:high"
```

## Step 3: Elaborate Top Issues

For high-priority issues missing detail:
- Acceptance criteria
- Approach sketch (direction, not full plan)
- Dependencies
- Risks

## Step 4: Research Open Questions

If issues need research before prioritization:
- Search documentation, check codebase, review existing implementations
- Document findings

## Step 5: Create New Issues

Capture ideas discovered during discussion:

```bash
gh issue create --title "[Type]: Description" --label "priority:X" --body "..."
gh project item-add <PROJECT_NUMBER> --owner <OWNER> --url <issue_url>
```

## Step 6: Select Sprint Scope

Select top 7 candidates by ICE score. Check `docs/constitution/PROCESS.md` MUST criteria:
- If any selected issue triggers a MUST escalation → notify stakeholder and wait
- Otherwise → proceed autonomously (no need to ask "which issues?")

Consider velocity from prior sprints (check `docs/sprints/velocity.md`).

## Step 6b: Define Sprint Goal

Define a one-sentence sprint goal:

Examples:
- "Establish core authentication module with tests"
- "Improve CI pipeline reliability and speed"
- "Add input validation across all API endpoints"

Label all sprint items with `sprint:N` (incrementing from last sprint).

## Step 7: Finalize Board

**MANDATORY**: Move all agreed items to Planned on the project board.

```bash
gh project item-edit --project-id PROJECT_ID --id ITEM_ID \
  --field-id STATUS_FIELD_ID --single-select-option-id PLANNED_OPTION_ID
```

Verify the move by listing the board.

## Step 8: Present Summary

```markdown
## Sprint N Planning — [Date]

### Sprint Goal
[One sentence theme]

### Sprint Backlog (agreed)
| # | Title | Priority | Est. Effort | Orchestration |
|---|-------|----------|-------------|---------------|

### Dependencies
- #Y depends on #X

### Board Changes
- Issues triaged: N
- Issues elaborated: N
- New issues created: N
- Moved to Planned: N

### Velocity Reference
Prior sprint: X issues in ~Yh (from docs/sprints/velocity.md)
```

After planning is complete, **proceed directly to sprint execution** (no separate start needed).
The sprint starts automatically per the stakeholder model. Send a summary notification.
