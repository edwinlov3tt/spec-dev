---
name: adr
description: "Create, review, or accept Architecture Decision Records. ADRs gate implementation — no phase starts without an accepted ADR."
arguments:
  - name: action
    description: "create <title> | accept <number> | list | review <number>"
    required: true
---

# /adr — Architecture Decision Records

ADRs are the binding specification for how things get built. They capture WHAT was decided, WHY, what alternatives were rejected, and what the acceptance criteria are.

## Actions

### `/adr create <title>`
Draft a new ADR. Follow this structure exactly:

```markdown
# ADR-NNNN: <title>

**Status:** Proposed
**Date:** <today>
**Deciders:** project owner
**Phase:** <which phase this ADR governs>

---

## Context
[What situation forces a decision? Why now?]

## Decisions
### Decision 1: <name>
[What was chosen, in concrete terms. Include code shapes if relevant.]

### Decision 2: <name>
[...]

## Implementation plan
[Ordered steps the implementer should follow]

## Acceptance criteria
[Numbered list of verifiable requirements]

## Alternatives considered
### Alt 1: <rejected approach>
[What it was. Why rejected.]

## Out of scope
[Explicitly what this ADR does NOT cover]

## Cross-links
[ADRs, research notes, handoffs this relates to]

## Notes
[Context, rationale, effort estimate]
```

Rules:
- Number sequentially from the last ADR in `docs/decisions/`
- Add to the index in `docs/decisions/README.md`
- ADRs are append-only — supersede, don't edit accepted ADRs
- Every Decision should be concrete enough for an implementer to build without guessing
- Acceptance criteria must be testable (not "works well" — specify what "works" means)

### `/adr accept <number>`
Accept an ADR:
1. Change `Status: Proposed` → `Status: Accepted`
2. Add `Accepted: <today's date>`
3. Update `docs/decisions/README.md` index
4. If the ADR has a corresponding phase in MASTER_PHASE_PLAN, update it

### `/adr list`
Read `docs/decisions/README.md` and show the index table.

### `/adr review <number>`
Read the ADR and provide structured feedback:
- Are the decisions concrete enough to implement?
- Are acceptance criteria testable?
- Are alternatives genuinely considered (not strawmen)?
- Are there contradictions with other accepted ADRs?
- What questions would an implementer have?

Output format:
```
ADR-NNNN Review:
- [OK/CONCERN] Decision 1: <assessment>
- [OK/CONCERN] Decision 2: <assessment>
- [MISSING] <anything not covered>
- Verdict: Ready to accept / Needs amendments
```

## Philosophy
- An ADR is NOT a research note (those capture design space without committing)
- An ADR is NOT a handoff doc (those tell an implementer what to build)
- An ADR IS a binding contract: once accepted, code must match it
- If reality conflicts with an accepted ADR, write an amendment — don't silently deviate
