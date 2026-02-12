# GitHub Copilot Instructions

This project uses **Copilot Scrum Autonomous** — an autonomous Scrum methodology where GitHub Copilot CLI acts as PO + Scrum Master.

For complete instructions, see `CLAUDE.md` in the repository root.

## Key Rules

- Follow the Definition of Done in `CLAUDE.md`
- Check `docs/constitution/PROCESS.md` for process rules
- Check `docs/architecture/ADR.md` for architectural decisions
- Use ICE scoring (Impact × Confidence / Effort) for prioritization
- Never merge without CI green
- Document huddles on issues and sprint log

## Sprint Commands

- `/sprint-planning` — Plan the next sprint
- `/sprint-start` — Begin sprint execution
- `/sprint-review` — Review sprint deliverables
- `/sprint-retro` — Run retrospective

## Agent Dispatch

Use custom agents (Sonnet) for code changes, tests, and reviews.
Use `task` agent (Haiku) only for running commands (build, test, lint).
