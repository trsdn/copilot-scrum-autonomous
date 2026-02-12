---
name: github-issues
description: 'Create, update, and manage GitHub issues using MCP tools. Use this skill when users want to create bug reports, feature requests, or task issues, update existing issues, add labels/assignees/milestones, or manage issue workflows. Triggers on requests like "create an issue", "file a bug", "request a feature", "update issue X", or any GitHub issue management task.'
---

# GitHub Issues

Manage GitHub issues using the `gh` CLI or MCP tools.

## Workflow

1. **Determine action**: Create, update, or query?
2. **Gather context**: Get repo info, existing labels, milestones if needed
3. **Structure content**: Use appropriate template
4. **Execute**: Call the appropriate tool
5. **Confirm**: Report the issue URL

## Creating Issues

### Using gh CLI

```bash
gh issue create \
  --title "[Type]: Description" \
  --label "priority:medium" \
  --body "## Description

[What needs to be done]

## Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2

## Notes

[Additional context]"
```

### Title Guidelines

- Start with type prefix when useful: `[Bug]`, `[Feature]`, `[Task]`
- Be specific and actionable
- Keep under 72 characters
- Examples:
  - `[Bug] Login fails with SSO enabled`
  - `[Feature] Add dark mode support`
  - `Add unit tests for auth module`

### Body Templates

#### Bug Report
```markdown
## Description
[What's broken]

## Steps to Reproduce
1. Step 1
2. Step 2

## Expected Behavior
[What should happen]

## Actual Behavior
[What happens instead]

## Environment
- OS:
- Version:
```

#### Feature Request
```markdown
## Summary
[What to build]

## Motivation
[Why this matters]

## Proposed Solution
[How to implement]

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
```

#### Task
```markdown
## Description
[What needs to be done]

## Approach
[How to do it]

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
```

## Updating Issues

```bash
# Add label
gh issue edit N --add-label "priority:high"

# Close with comment
gh issue comment N --body "Completed in PR #M"
gh issue close N

# Add to project board
gh project item-add <PROJECT_NUMBER> --owner <OWNER> --url <issue_url>
```

## Common Labels

| Label | Use For |
|-------|---------|
| `bug` | Something isn't working |
| `enhancement` | New feature or improvement |
| `documentation` | Documentation updates |
| `priority:high` | ICE score ≥ 4 |
| `priority:medium` | ICE score 2-3 |
| `priority:low` | ICE score < 2 |
| `needs:stakeholder-decision` | Requires stakeholder input |
| `sprint:N` | Assigned to sprint N |

## Tips

- Always confirm the repository context before creating issues
- Ask for missing critical information rather than guessing
- Link related issues when known: `Related to #123`
- For updates, fetch current issue first to preserve unchanged fields
