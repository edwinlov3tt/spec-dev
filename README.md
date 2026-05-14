# spec-driven-dev

A Claude Code plugin for spec-driven software development. ADR-first architecture, phase sequencing, self-audit, multi-instance orchestration.

> Built from the development methodology used to ship Mosaic — a 10-crate Rust platform with 90+ test suites, 27 ADRs, and 15+ phases shipped using this exact process.

## Philosophy

**Spec before code. Always.**

Every feature follows this lifecycle:
```
Research note → ADR → Handoff → Implement → Audit → Accept → Ship
```

No phase starts without an accepted ADR. No implementation starts without a handoff. No phase ships without a self-audit. This isn't bureaucracy — it's how you ship complex systems with AI instances that don't share memory.

## Commands

| Command | What it does |
|---|---|
| `/onboard` | Read project docs and synthesize a briefing for the current session |
| `/phase status` | Show current/next/recent phases from the master plan |
| `/phase propose <name>` | Add a new phase to the master plan |
| `/phase complete <id>` | Mark a phase as complete |
| `/phase next` | What should be worked on next |
| `/adr create <title>` | Draft a new Architecture Decision Record |
| `/adr accept <number>` | Accept an ADR (status → Accepted) |
| `/adr review <number>` | Structured review with verdict |
| `/adr list` | Show the ADR index |
| `/handoff <phase>` | Generate implementation handoff + prompt |
| `/audit <phase>` | Run 8-section self-audit on a completed phase |
| `/sync` | Extract decisions/issues/state from conversation into docs |
| `/push` | Safe commit: secrets scan + gates + conventional commit |
| `/review <doc>` | Structured review for dual-review pattern |
| `/research <topic>` | Create a pre-ADR research note |

## Agents

| Agent | Role |
|---|---|
| `architect` | Design phases, write ADRs, plan sequences. Never writes code. |
| `auditor` | Self-audit completed phases. Always a different instance than the implementer. |

## Skills

| Skill | What it does |
|---|---|
| `claude-md-template` | Generate a CLAUDE.md operating manual for a new project |
| `process-setup` | Scaffold the full spec-driven process (directories, master plan, templates) |

## Hooks

| Hook | Trigger | What it does |
|---|---|---|
| `pre-commit-secrets` | Before `git commit` | Scans staged diff for API keys, tokens, passwords. Blocks if found. |

## Installation

```bash
# Clone into your Claude Code plugins directory
git clone https://github.com/edwinlov3tt/spec-driven-dev.git ~/.claude/plugins/spec-driven-dev
```

Or add to your project's `.claude/plugins/`:
```bash
cd your-project
mkdir -p .claude/plugins
git clone https://github.com/edwinlov3tt/spec-driven-dev.git .claude/plugins/spec-driven-dev
```

## Quick start

```bash
# 1. Set up the process infrastructure
/process-setup

# 2. Onboard to the project
/onboard

# 3. Create your first phase
/adr create "Phase 1 — Core foundation"

# 4. Accept and implement
/adr accept 1
/handoff 1
# Give the prompt to an implementing instance

# 5. Audit and ship
/audit 1
/phase complete 1
```

## The development loop

```
     ┌──────────────┐
     │  /research    │  ← explore design space
     └──────┬───────┘
            ▼
     ┌──────────────┐
     │  /adr create  │  ← commit to approach
     └──────┬───────┘
            ▼
     ┌──────────────┐
     │  /review      │  ← dual-review (multiple AI perspectives)
     └──────┬───────┘
            ▼
     ┌──────────────┐
     │  /adr accept  │  ← lock the spec
     └──────┬───────┘
            ▼
     ┌──────────────┐
     │  /handoff     │  ← generate implementation prompt
     └──────┬───────┘
            ▼
     ┌──────────────┐
     │  Implement    │  ← separate instance builds it
     └──────┬───────┘
            ▼
     ┌──────────────┐
     │  /audit       │  ← yet another instance audits it
     └──────┬───────┘
            ▼
     ┌──────────────┐
     │  /push        │  ← safe commit + push
     └──────┬───────┘
            ▼
     ┌──────────────┐
     │ /phase complete│  ← update master plan
     └──────┬───────┘
            ▼
     ┌──────────────┐
     │  /sync        │  ← extract session knowledge
     └──────────────┘
```

## Why this works

1. **Docs are load-bearing.** ADRs gate implementation. Handoffs ARE the prompt. Completion reports close phases. You can't skip them because the next instance can't work without them.

2. **Multiple perspectives catch issues early.** The dual-review pattern (Desktop + GPT + Claude PM) catches architectural issues before they become code.

3. **Phases are sequential and bounded.** One thing at a time. Clear scope. Clear acceptance criteria. No "while I'm here, let me also..."

4. **Self-audit prevents drift.** A different instance audits the work. CRIT findings block acceptance. This is how you maintain quality across AI handoffs.

5. **Knowledge survives sessions.** `/sync` extracts decisions into persistent docs. `/onboard` reads them back. No "what were we doing?" at the start of each session.

## License

MIT
