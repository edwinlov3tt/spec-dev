---
name: phase
description: "Manage the MASTER_PHASE_PLAN — view current state, propose new phases, mark phases complete, resequence."
arguments:
  - name: action
    description: "status | propose <name> | complete <phase-id> | next"
    required: true
---

# /phase — Phase Management

Manages the project's MASTER_PHASE_PLAN.md — the single source of truth for what phase the project is in and what comes next.

## Actions

### `/phase status`
Read `docs/roadmap/MASTER_PHASE_PLAN.md` and report:
- Current phase (the one marked `in progress` or `proposed`)
- Recently completed phases (last 3)
- Next planned phases (next 3)
- Any phases marked `planned (needs ADR)` — flag these as blocked

### `/phase propose <name>`
Propose a new phase. Before adding it to the plan:
1. Check if an ADR exists for this scope. If not, note "needs ADR."
2. Determine where it sequences relative to existing phases.
3. Draft a one-line description in the MASTER_PHASE_PLAN format:
   `| **<ID>** | <description> | planned | — |`
4. Show the user the proposed entry and ask for approval before editing.

Rules:
- Never invent a phase number that conflicts with an existing one
- Sub-phases use dot notation (e.g., 7A.7, 8.1, 5D.1)
- Every phase needs: name, description, status, and a tag column
- Link the ADR if one exists

### `/phase complete <phase-id>`
Mark a phase as complete in the master plan:
1. Read the current entry
2. Update status to `**complete**`
3. Add the git tag or commit hash
4. Update the "Last updated" header line with today's date and a summary
5. Show the diff to the user

### `/phase next`
Determine what should be worked on next:
1. Read the master plan
2. Find the first `planned` or `proposed` phase
3. Check if it has an ADR (if "needs ADR" → that's the first step)
4. Check if it has a handoff doc
5. Report: "Next phase is **X**. It {has/needs} an ADR. It {has/needs} a handoff."

## Rules
- MASTER_PHASE_PLAN.md is the single source of truth
- At most one phase should be `in progress` at a time
- `proposed` means "next to start" — at most one row
- `planned` means committed but not yet active
- Never delete a completed phase — history is preserved
- Phase numbering follows the existing convention in the plan
