---
name: process-setup
description: "Set up the full spec-driven development process for a new or existing project — directory structure, master plan, process notes, templates."
---

# Process Setup

When the user wants to adopt the spec-driven development process, scaffold the full infrastructure.

## What to create

### 1. Directory structure

```
docs/
├── decisions/           # ADRs — one file per decision
│   └── README.md        # Index table of all ADRs
├── research-notes/      # Pre-ADR design exploration
├── handoffs/            # Implementation handoff documents
├── reports/             # Phase completion reports
├── roadmap/
│   └── MASTER_PHASE_PLAN.md  # Single source of truth for phases
├── specs/               # Specification documents
├── strategy/            # Strategic/vision documents
├── security/            # Security posture and threat model
├── for-dummies/         # Plain-language explanations of phases
│   └── phases/
├── templates/           # Document templates
│   ├── adr.md
│   ├── handoff.md
│   ├── research-note.md
│   └── phase-completion-report.md
└── process-notes.md     # Development rules and conventions
```

### 2. MASTER_PHASE_PLAN.md (starter)

```markdown
# MASTER_PHASE_PLAN

> The single source of truth for what phase the project is in and what comes next.

**Last updated:** <today>

---

## Phase status overview

| Phase | Name | Status | Tag |
|---|---|---|---|
| **1** | [First phase] | planned | — |

**Status legend:**
- **complete** — shipped and tagged
- **proposed** — next to start (at most one at a time)
- **planned** — committed but not yet active
- **not started** — no scoping yet
```

### 3. decisions/README.md (starter)

```markdown
# decisions/

Architecture Decision Records (ADRs). One file per decision.

## Index

| ADR | Title | Status |
|---|---|---|
```

### 4. process-notes.md (starter rules)

```markdown
# Process Notes

## Rule 1: Spec-driven development
Every implementation phase requires an accepted ADR before code is written.

## Rule 2: Phase sequencing
MASTER_PHASE_PLAN.md is the single source of truth. Nothing starts without being listed.

## Rule 3: Self-audit
Every phase completion triggers a structured self-audit (8 sections, A–H).

## Rule 4: No speculative implementation
Research notes and design sketches can happen early. Production code waits for its predecessor phase to ship.

## Rule 5: Dual-review for cross-cutting decisions
Architectural decisions that span phases benefit from multiple reviewer perspectives.
```

### 5. CLAUDE.md

Generate using the `/claude-md-template` skill, customized for the project.

## After setup

Tell the user:
```
Process infrastructure is ready. Here's how to start:

1. Define Phase 1 scope → /adr create "Phase 1 — <scope>"
2. Accept the ADR → /adr accept 1
3. Generate the handoff → /handoff 1
4. Hand the prompt to an implementing instance
5. When done → /audit 1
6. Mark complete → /phase complete 1
7. Repeat from step 1 for Phase 2
```
