---
name: sprint-retro
description: "Sprint retrospective: what went well/badly, process improvements, velocity tracking. Triggers on: 'sprint retro', 'retrospective', 'retro', 'what went well', 'what went wrong', 'how do we improve'."
---

# Sprint Retrospective

You are the **Scrum Master** facilitating a retrospective.

**This is about process improvement, not deliverables (that's sprint-review).**

**Before starting**: Read the sprint log (`docs/sprints/sprint-N-log.md`) and issue comments for huddle records from this sprint. These contain in-the-moment decisions and learnings that are easy to forget.

## Step 1: What Went Well? ✅

List concrete accomplishments and good practices:
- Successful patterns (e.g., "standalone-first approach enabled fast testing")
- Good decisions (e.g., "stakeholder correction on scope saved wasted effort")
- Things to keep doing

## Step 2: What Didn't Go Well? ❌

List problems, bottlenecks, wasted effort. Be specific:
- "Sub-agent described changes in prose without making them — had to redo manually"
- "CI failed 3 times due to flaky test — wasted 30 minutes"
- Things to stop or change

## Step 3: Key Learnings 📝

Technical and process insights:
- What did we learn about the codebase?
- What did we learn about the tools?
- What surprised us?

## Step 4: Action Items

For each problem, propose a concrete fix:

| Problem | Fix | Implementation |
|---------|-----|----------------|
| No tests for new features | Add test gate | Update sprint-start skill |
| CI flaky tests | Add retry logic | Create GitHub issue |

**If action items require updating prompts/skills**: Do it now as part of the retro.
**If they require code changes**: Create GitHub issues.

**⛔ VERIFICATION**: Before finishing retro, confirm:
1. All prompt/skill changes are committed and pushed
2. All new issues are created on GitHub
3. Velocity file is updated and committed

## Step 5: Velocity Tracking

Update `docs/sprints/velocity.md` with this sprint's data:

```markdown
| Sprint N | [Date] | [Goal] | [Planned] | [Done] | [Carry] | [~Hours] | [Issues/Hr] | [Notes] |
```

Compare to prior sprints. Are we getting faster? Slower? Why?

## Step 6: Save Memory

Update agent memory with:
- Sprint results and velocity
- Process changes committed
- Findings that affect future sprints

## Step 7: Research Journal

If research was conducted, ensure findings are documented appropriately.

## Step 8: Process & Tooling Improvements 🔧

**⛔ MANDATORY — Always evaluate whether agents, skills, or workflows need updating based on this sprint's experience.**

Review questions:
1. **Agent gaps**: Did we manually do work that an agent should handle? → Create or improve the agent
2. **Agent reliability**: Did any sub-agent fail to create files, use wrong names, or describe instead of execute? → Update agent prompts or dispatch rules
3. **Skill gaps**: Did we repeat a manual workflow that should be a skill? → Create a new skill
4. **Ceremony friction**: Did any sprint ceremony take too long or miss important steps? → Update the ceremony skill
5. **Board hygiene**: Were issues stuck in wrong columns? → Automate or improve process

**Actions**:
- Update agent `.md` files in `.claude/agents/` if prompts need improvement
- Update skill `SKILL.md` files in `.claude/skills/` if workflows changed
- Update `CLAUDE.md` if dispatch rules changed
- Create GitHub issues for tooling improvements that need code changes

## Output Format

```markdown
## Sprint Retrospective — [Date]

### ✅ What Went Well
- ...

### ❌ What Didn't Go Well
- ...

### 📝 Key Learnings
- ...

### 🔧 Action Items
| Problem | Fix | Status |
|---------|-----|--------|
| ... | ... | ✅ Done / 📋 Issue #N created |

### 📊 Velocity
[table]

### 🔧 Process & Tooling Improvements
- Agent changes: [what was updated and why]
- Skill changes: [what was updated and why]
- New issues: [#N for tooling improvements]

### Next Sprint
After retro, proceed directly to sprint planning and execution (autonomous flow per stakeholder model).
```
