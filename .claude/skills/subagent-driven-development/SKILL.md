---
name: subagent-driven-development
description: "Use when executing implementation plans with independent tasks in the current session. Triggers on: 'use subagents', 'dispatch subagents', 'subagent development', 'execute with subagents', 'execute the plan'. REQUIRES a user-approved plan before starting — never begin without explicit user consent."
---

# Subagent-Driven Development

Execute plan by dispatching fresh subagent per task, with two-stage review after each: spec compliance review first, then code quality review.

**Core principle:** Fresh subagent per task + two-stage review (spec then quality) = high quality, fast iteration

## When to Use

- Have an implementation plan with mostly independent tasks
- Want to stay in the current session
- Need quality gates between tasks

## When NOT to Use

- Tasks are tightly coupled and need shared context
- No implementation plan exists (use `writing-plans` first)
- Exploratory/prototyping work

## The Process

```
1. Read plan, extract all tasks with full text
2. Create todos for all tasks

Per task:
3. Dispatch implementer subagent with full task text + context
4. Implementer implements, tests, commits, self-reviews
5. Dispatch spec reviewer subagent
6. If spec issues → implementer fixes → re-review
7. Dispatch code quality reviewer subagent
8. If quality issues → implementer fixes → re-review
9. Mark task complete

After all tasks:
10. Dispatch final code reviewer for entire implementation
11. Finish development branch
```

## Rules

**Never:**
- Skip reviews (spec compliance OR code quality)
- Proceed with unfixed issues
- Dispatch multiple implementation subagents in parallel (conflicts)
- Make subagent read plan file (provide full text instead)
- Skip review loops (reviewer found issues = implementer fixes = review again)
- **Start code quality review before spec compliance is ✅** (wrong order)
- Move to next task while either review has open issues

**If subagent asks questions:**
- Answer clearly and completely
- Provide additional context if needed

**If reviewer finds issues:**
- Implementer (same subagent) fixes them
- Reviewer reviews again
- Repeat until approved

**If subagent fails task:**
- Dispatch fix subagent with specific instructions
- Don't try to fix manually (context pollution)

## Advantages

**vs. Manual execution:**
- Subagents follow TDD naturally
- Fresh context per task (no confusion)
- Parallel-safe (subagents don't interfere)

**vs. Parallel session:**
- Same session (no handoff)
- Continuous progress (no waiting)
- Review checkpoints automatic

**Quality gates:**
- Self-review catches issues before handoff
- Two-stage review: spec compliance, then code quality
- Review loops ensure fixes actually work
- Spec compliance prevents over/under-building
