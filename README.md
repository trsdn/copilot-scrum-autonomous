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
- **Agent Dispatch** — Sonnet for code/tests/review, Haiku for commands/search
- **Subagent-Driven Development** — Fresh subagent per task with two-stage review
- **Daily Huddles** — Documented on issues + sprint log after each task
- **Verification Before Completion** — "Evidence before claims, always"
- **Agent Memory** — Persistent knowledge across sessions
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
| `CLAUDE.md` | Replace `{{PROJECT_NAME}}` and `{{PROJECT_DESCRIPTION}}` placeholders |
| `CLAUDE.md` | Add your project-specific commands, coding conventions, key files |
| `docs/architecture/ADR.md` | Add your architectural decisions |
| `.claude/agents/*.md` | Customize agent expertise for your domain |
| `.claude/settings.json` | Update allowed commands for your tooling |
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

Update the project board URL in `CLAUDE.md`.

### 5. Start Your First Sprint

```bash
# Open GitHub Copilot CLI in your project
claude

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

| Task | Agent | Model |
|------|-------|-------|
| Code changes | `code-developer` | Sonnet |
| Writing tests | `test-engineer` | Sonnet |
| Code review | `code-review` | Sonnet |
| Research/docs | `research-agent` / `documentation-agent` | Sonnet |
| Quick file search | `explore` | Haiku |
| Build/test commands | `task` | Haiku |

> ⛔ **Never** use the generic `task` agent (Haiku) for code changes. It lacks reasoning capacity for multi-file edits.

## Directory Structure

```
├── .claude/
│   ├── settings.json          # Permissions, hooks, skills & agents registry
│   ├── hooks/
│   │   ├── ntfy-notify.sh     # Push notification hook
│   │   └── session-reminder.sh # Session start reminder
│   ├── skills/                # Sprint ceremonies + workflow skills
│   │   ├── sprint-planning/
│   │   ├── sprint-start/
│   │   ├── sprint-review/
│   │   ├── sprint-retro/
│   │   ├── agent-memory/
│   │   ├── notify/
│   │   ├── code-review/
│   │   ├── create-pr/
│   │   ├── tdd-workflow/
│   │   ├── verification-before-completion/
│   │   ├── writing-plans/
│   │   ├── subagent-driven-development/
│   │   └── github-issues/
│   └── agents/                # Specialized agent definitions
│       ├── code-developer.md
│       ├── test-engineer.md
│       ├── documentation-agent.md
│       ├── security-reviewer.md
│       └── research-agent.md
├── .github/
│   ├── workflows/
│   │   ├── ci.yml             # CI: lint, typecheck, test, security
│   │   └── release.yml        # Semantic release
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.yml
│   │   ├── feature_request.yml
│   │   └── config.yml
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── copilot-instructions.md
├── docs/
│   ├── constitution/
│   │   └── PROCESS.md         # Development process constitution
│   ├── architecture/
│   │   └── ADR.md             # Architectural Decision Records
│   ├── sprints/
│   │   └── velocity.md        # Sprint velocity tracking
│   └── plans/
│       └── .gitkeep
├── CLAUDE.md                   # Main Copilot instructions
├── Makefile                    # Common development targets
├── pyproject.toml              # Python project configuration
└── .gitignore
```

## Customization Guide

### Adding Domain-Specific Agents

Create `.claude/agents/your-agent.md`:

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

Register in `.claude/settings.json` under `agents`.

### Adding New Skills

Create `.claude/skills/your-skill/SKILL.md`:

```yaml
---
name: your-skill
description: "When to use this skill"
---
```

Register in `.claude/settings.json` under `skills`.

### Language Adaptation

This template defaults to Python tooling. To adapt for other languages:

1. Update `Makefile` targets with your build/test/lint commands
2. Update `.claude/settings.json` permissions for your tooling
3. Update `.github/workflows/ci.yml` for your CI pipeline
4. Adjust coding conventions in `CLAUDE.md`
5. Update TDD skill templates for your test framework

## Philosophy

> **Focus, Quality, Incremental, Improve** — in that order.

1. **Protect Focus** — Complete what you start before moving on
2. **Quality Gates** — Every change is tested, reviewed, and verified
3. **Small, Testable Diffs** — One feature per PR (~150 lines ideal)
4. **Continuous Improvement** — Every retro produces actionable improvements

## License

MIT
