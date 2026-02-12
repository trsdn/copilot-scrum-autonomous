# Copilot Scrum Autonomous

[![Optimized for GitHub Copilot CLI](https://img.shields.io/badge/Optimized%20for-GitHub%20Copilot%20CLI-blue?logo=github)](https://docs.github.com/en/copilot)

A template repository for **autonomous Scrum development** powered by GitHub Copilot CLI. The AI agent acts as both **Product Owner and Scrum Master**, while the human serves as a **Stakeholder** with veto rights.

> **This is the AUTONOMOUS variant.** For the PO-driven variant where the human drives each sprint phase manually, see [copilot-scrum-guided](https://github.com/trsdn/copilot-scrum-guided).

---

## What This Is

A production-tested methodology for running full Scrum sprints with GitHub Copilot CLI as the autonomous driver. The agent plans sprints, executes issues, reviews deliverables, and runs retrospectives — only escalating to the human for strategic decisions.

This template is **domain-agnostic**. It was extracted from a real production workflow and works for any software project using GitHub Copilot CLI.

## The Operating Model

| Role | Who | Responsibility |
|------|-----|----------------|
| **Stakeholder** | Human | Strategic direction, veto right, escalation decisions |
| **PO + Scrum Master** | Copilot Agent | Backlog management, sprint execution, quality gates, ceremonies |

The agent operates autonomously through full sprint cycles. The stakeholder is only involved when escalation criteria are met.

### Escalation Model

| Level | When | Examples |
|-------|------|----------|
| ⛔ **MUST Escalate** | Strategic, ADR, constitution, dependencies, production, spending | "Should we add a new framework?" |
| ⚠️ **SHOULD Escalate** | Scope > 8, deprioritize high, big refactor, close >5 stale | "Planning 9 issues, OK?" |
| ✅ **Autonomous** | Everything else | Sprint planning, code, tests, config, docs, CI |

## Sprint Cycle

```
Planning → Start → [Execute with Huddles] → Review → Retro → repeat
```

| Phase | Slash Command | What Happens |
|-------|---------------|--------------|
| **Planning** | `/sprint-planning` | Triage backlog, ICE score, select scope, finalize board |
| **Start** | `/sprint-start` | Create sprint log, begin execution (no consent gate) |
| **Execute** | *(automatic)* | Issue by issue with quality gates and huddles |
| **Review** | `/sprint-review` | Gather evidence, delivery report, notify stakeholder |
| **Retro** | `/sprint-retro` | What went well/badly, velocity, process improvements |

Sprints flow automatically — planning leads directly into execution without manual intervention.

## Key Features

- **ICE Scoring** — Impact × Confidence / Effort for prioritization
- **Quality Gates** — Test gate (≥3 tests/feature), CI gate, Definition of Done
- **Agent Dispatch** — Specialized agents for code/tests/review, lightweight agents for commands/search
- **Daily Huddles** — Documented on issues + sprint log after each task
- **Verification Before Completion** — "Evidence before claims, always"
- **Notifications** — Push notifications via [ntfy.sh](https://ntfy.sh)
- **Velocity Tracking** — Sprint-over-sprint performance data

## Getting Started

### 1. Use as Template

Click **"Use this template"** on GitHub, or:

```bash
gh repo create my-project --template trsdn/copilot-scrum-autonomous --clone
cd my-project
```

### 2. Customize for Your Project

| File | What to Change |
|------|----------------|
| `AGENTS.md` | Replace `{{PROJECT_NAME}}` and `{{PROJECT_DESCRIPTION}}` placeholders |
| `AGENTS.md` | Add your project-specific commands, coding conventions, key files |
| `docs/architecture/ADR.md` | Add your architectural decisions |
| `.github/agents/*.agent.md` | Customize agent expertise for your domain |
| `.github/copilot-instructions.md` | Adjust Copilot behavior and conventions |
| `Makefile` | Add project-specific targets |
| `pyproject.toml` | Configure your project metadata |
| `.github/workflows/ci.yml` | Adjust CI for your language/framework |

### 3. Set Up Notifications (Optional)

```bash
# Install ntfy on your phone (iOS/Android)
# Subscribe to a secret topic

echo 'export NTFY_TOPIC="your-secret-topic"' >> ~/.zshrc
source ~/.zshrc

# Test
curl -d "Hello from Copilot!" ntfy.sh/$NTFY_TOPIC
```

### 4. Set Up Project Board

Create a GitHub Project board with these columns:

```
Ideas → Backlog → Planned → In Progress → Validation → Done
```

Update the project board URL in `AGENTS.md`.

### 5. Start Your First Sprint

```bash
# Open GitHub Copilot CLI in your project
copilot

# Run sprint planning
/sprint-planning

# Execution starts automatically after planning
```

## How It Works

### The Sprint Cycle in Detail

1. **Planning** (`/sprint-planning`)
   - Reviews board state and open issues
   - Triages unlabeled issues with ICE scoring
   - Elaborates top issues with acceptance criteria
   - Selects sprint scope (~7 issues based on velocity)
   - Moves items to "Planned" on the board
   - Proceeds directly to execution

2. **Execution** (automatic)
   - Works through issues one at a time
   - Creates worktree per issue, implements, tests, PRs
   - Runs daily huddle after each issue (documented on issue + sprint log)
   - Quality gates: tests (≥3/feature), CI green, Definition of Done

3. **Review** (`/sprint-review`)
   - Gathers evidence (commits, PRs, closed issues)
   - Creates delivery report with metrics
   - Accepts deliverables autonomously (unless MUST criteria met)
   - Sends sprint summary notification

4. **Retro** (`/sprint-retro`)
   - What went well / what didn't
   - Key learnings and action items
   - Velocity tracking update
   - **Process & tooling improvements** (mandatory Step 8)

### Agent Dispatch

| Task | Agent | Use Case |
|------|-------|----------|
| Code changes | `@code-developer` | Multi-file reasoning, refactoring |
| Writing tests | `@test-engineer` | Behavior understanding, TDD |
| Code review | `@code-review` | Structured review with checklists |
| Research/docs | `@research-agent` / `@documentation-agent` | Synthesis, technical writing |

## Sprint Documentation & Artifacts

Every sprint produces structured documentation that creates an audit trail and preserves knowledge across sessions.

### Where Things Are Stored

| Artifact | Location | Created By | Purpose |
|----------|----------|------------|---------|
| Sprint log | `docs/sprints/sprint-N-log.md` | Sprint Start | Huddle decisions, learnings, plan changes during execution |
| Velocity data | `docs/sprints/velocity.md` | Sprint Retro | Sprint-over-sprint performance tracking |
| Issue comments | GitHub Issues | Huddles | Traceable audit trail per issue |
| Implementation plans | `docs/plans/` | Planning / Writing Plans | Detailed implementation specs |
| ADRs | `docs/architecture/ADR.md` | As needed | Immutable architectural decisions |
| Process rules | `docs/constitution/PROCESS.md` | Sprint Retro | Evolving process constitution |

### The Huddle Documentation Rule

After each issue is completed, a **daily huddle** is performed and documented in **two places**:

1. **Comment on the completed GitHub issue** — creates a traceable, permanent record:
   ```bash
   gh issue comment 42 --body "### Huddle — Sprint 5, Issue 3/7 done
   **Outcome**: Implemented rate limiter with token bucket, 95% test coverage
   **Key learning**: Redis connection pooling needed for production scale
   **Decision**: Re-prioritize #45 above #43 based on this finding
   **Next**: #45 — Connection pool configuration"
   ```

2. **Append to sprint log** (`docs/sprints/sprint-N-log.md`) — preserves context for retros:
   ```markdown
   ### Huddle — After Issue #42 (2025-01-15 14:30)
   **Completed**: #42 — Rate limiter implemented
   **Sprint progress**: 3/7 issues done
   **Key learning**: Redis pooling impacts performance at scale
   **Plan check**: Reordered — #45 now before #43
   **Next up**: #45 — Connection pool configuration
   ```

### Sprint Log Template

Created automatically at sprint start (`docs/sprints/sprint-N-log.md`):

```markdown
# Sprint N Log — [Date]

**Goal**: [One-sentence sprint goal]
**Planned**: [N] issues

## Huddles
[Appended after each issue completes]
```

### Velocity Tracking

Updated each sprint retro in `docs/sprints/velocity.md`:

```markdown
| Sprint | Date | Goal | Planned | Done | Carry | ~Hours | Issues/Hr | Notes |
|--------|------|------|---------|------|-------|--------|-----------|-------|
| 1      | ...  | ...  | 7       | 7    | 0     | 3.0    | 2.3       | First sprint |
| 2      | ...  | ...  | 7       | 5    | 2     | 3.5    | 1.4       | Integration heavy |
```

This data drives sprint sizing — the agent uses historical velocity to determine how many issues to plan.

## Directory Structure

```
├── AGENTS.md                        # Project-specific agent instructions
├── .github/
│   ├── copilot-instructions.md      # Main Copilot instructions
│   ├── agents/                      # Specialized agent definitions
│   │   ├── code-developer.agent.md
│   │   ├── test-engineer.agent.md
│   │   ├── documentation-agent.agent.md
│   │   ├── security-reviewer.agent.md
│   │   └── research-agent.agent.md
│   ├── prompts/                     # Reusable workflow prompts
│   │   ├── sprint-planning.prompt.md
│   │   ├── sprint-start.prompt.md
│   │   ├── sprint-review.prompt.md
│   │   ├── sprint-retro.prompt.md
│   │   ├── orchestrate-feature.prompt.md
│   │   ├── orchestrate-bugfix.prompt.md
│   │   ├── code-review.prompt.md
│   │   ├── create-pr.prompt.md
│   │   └── tdd-workflow.prompt.md
│   ├── workflows/
│   │   ├── ci.yml                   # CI: lint, typecheck, test, security
│   │   └── release.yml              # Semantic release
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.yml
│   │   ├── feature_request.yml
│   │   └── config.yml
│   └── PULL_REQUEST_TEMPLATE.md
├── docs/
│   ├── constitution/
│   │   └── PROCESS.md               # Development process constitution
│   ├── architecture/
│   │   └── ADR.md                   # Architectural Decision Records
│   ├── sprints/
│   │   └── velocity.md              # Sprint velocity tracking
│   └── plans/
│       └── .gitkeep
├── scripts/copilot-notify.sh        # Push notification script
├── Makefile                          # Common development targets
├── pyproject.toml                    # Python project configuration
└── .gitignore
```

## Customization Guide

### Adding Agents

Create `.github/agents/your-agent.agent.md`:

```markdown
# Agent: Your Domain Expert

## Role
[What this agent specializes in]

## Capabilities
- [Capability 1]
- [Capability 2]

## Tools
- [Available tools]

## Guidelines
- [Domain-specific rules]
```

Agents in `.github/agents/` are automatically discovered by GitHub Copilot CLI.

### Adding Prompts

Create `.github/prompts/your-prompt.prompt.md`:

```markdown
---
description: "Short description of when to use this prompt"
---

# Your Workflow Prompt

## Steps

1. [Step 1]
2. [Step 2]
3. [Step 3]
```

Prompts in `.github/prompts/` are available as slash commands in GitHub Copilot CLI.

### Language Adaptation

This template defaults to Python tooling. To adapt for other languages:

1. Update `Makefile` targets with your build/test/lint commands
2. Update `.github/copilot-instructions.md` for your language conventions
3. Update `.github/workflows/ci.yml` for your CI pipeline
4. Adjust coding conventions in `AGENTS.md`
5. Update TDD prompt templates for your test framework

### Customization Reference

| What | Where | Purpose |
|------|-------|---------|
| Project-specific instructions | `AGENTS.md` | Commands, conventions, repo structure |
| Copilot behavior & process | `.github/copilot-instructions.md` | Sprint process, escalation, DoD |
| Specialized agents | `.github/agents/*.agent.md` | Domain-specific agent roles |
| Workflow prompts | `.github/prompts/*.prompt.md` | Reusable slash commands |
| CI pipeline | `.github/workflows/ci.yml` | Build, test, lint automation |

## Philosophy

> **Focus, Quality, Incremental, Improve** — in that order.

1. **Protect Focus** — Complete what you start before moving on
2. **Quality Gates** — Every change is tested, reviewed, and verified
3. **Small, Testable Diffs** — One feature per PR (~150 lines ideal)
4. **Continuous Improvement** — Every retro produces actionable improvements

## License

MIT
