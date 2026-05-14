---
name: onboard
description: "Read all project docs in priority order and synthesize a briefing for the current session. Use at the start of every new conversation."
arguments:
  - name: focus
    description: "Optional focus area (e.g., 'phase-8', 'narrative-engine', 'security')"
    required: false
---

# /onboard — Project Orientation

You are onboarding to a project. Read the following documents in priority order, then synthesize a briefing. Do NOT skip steps. Do NOT start coding until this is complete.

## Phase 1: Read (in this exact order)

1. **CLAUDE.md** (root) — operating manual, forbidden patterns, strict requirements
2. **docs/roadmap/MASTER_PHASE_PLAN.md** — what phase we're in, what's next
3. **docs/CURRENT_STATE.md** (if it exists) — latest snapshot of project state
4. **HANDOFF.md** (if it exists) — most recent handoff document
5. **The ADR for the current/next phase** — binding decisions for the work ahead
6. **docs/process-notes.md** — development rules, workflow conventions
7. **Recent git log** (`git log --oneline -20`) — what shipped recently

If `$ARGUMENTS` includes a focus area, also read:
- Any ADR, handoff, or research note matching that focus
- Any completion report for that phase

## Phase 2: Synthesize

Output a structured briefing in this format:

```
PROJECT: [name]
PHASE: [current phase from MASTER_PHASE_PLAN]
STATUS: [what's complete, what's in progress, what's next]

KEY CONSTRAINTS:
- [list binding rules from CLAUDE.md that affect current work]

ACTIVE WORK:
- [what's being built right now, with file paths]

BLOCKING ISSUES:
- [any known issues, debt, or unresolved questions]

RECOMMENDED NEXT STEP:
- [what to do first in this session]
```

## Phase 3: Confirm

Ask the user: "Ready to proceed with [recommended next step], or do you want to focus on something else?"

Do NOT:
- Start writing code before completing the briefing
- Scan the entire codebase (read docs, not source files)
- Make assumptions about project state without reading the docs
- Skip CLAUDE.md — it contains binding rules that override your defaults
