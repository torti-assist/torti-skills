# Torti Skills Repository

OpenClaw AI companion skill development and management.

## Overview

This repository manages Torti's custom skills, validations, and continuous development using ClawSweeper for issue triage and code review.

## Structure

- `skills/` - Custom SKILL.md implementations
- `issues/` - GitHub issues tracking feature requests, bugs, improvements
- `records/` - ClawSweeper audit trail & decisions
- `docs/` - Skill documentation & guides

## Skill Development Workflow

1. **Daily Review** (06:00 UTC) - ClawSweeper scans open issues/PRs
2. **Weekly Planning** (Monday 09:00 UTC) - New development tickets created
3. **Implementation** - Skills developed with git versioning
4. **Validation** (Bi-weekly Thursday 14:00 UTC) - Tests run, results reviewed
5. **Integration** - Approved skills merged to main

## Models & Token Budget

- **Primary:** openrouter/nvidia/nemotron-3-super-120b-a12b (FREE tier)
- **Fallback:** google/gemini-3.1-flash-lite-preview (FREE tier)
- **Local:** ollama/gemma-4-E2B-it-UD-IQ2_M (unlimited, 2B)
- **ClawSweeper:** Codex (gpt-5.5) with 10-min timeouts

## Recent Changes

- 2026-05-14: ClawSweeper installed & configured
- 2026-05-14: Health check completed, security audit passed
- 2026-05-14: Project structure initialized

## License

Part of OpenClaw personal assistant project
