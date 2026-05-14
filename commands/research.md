---
name: research
description: "Create a research note capturing design space for a future feature. Research notes are pre-ADR — they explore options without committing."
arguments:
  - name: topic
    description: "Topic for the research note (e.g., 'service-daemon', 'grout-security')"
    required: true
---

# /research — Research Note

Create a research note at `docs/research-notes/<topic>.md`. Research notes capture design space — what could be built, how it might work, what questions need answers — without committing to implementation.

## Structure

```markdown
# <Topic> — <Subtitle>

**Status:** Research note (pre-ADR)
**Date:** <today>
**Scope:** <what this note covers>
**Sequencing:** <when this might become an ADR/phase>

---

## The problem
[What gap or opportunity does this address?]

## Proposed approach
[How it might work — with enough detail to evaluate feasibility]

## Design questions
[Numbered list of decisions that need to be made before an ADR can draft]

## Alternatives considered
[Other approaches and why they might or might not work]

## What this enables
[Why it matters — what becomes possible after this ships]

## Implementation sketch
[Rough crate structure, key types, major code sites]

## Open questions for the project owner
[Decisions that need human judgment, not just engineering analysis]

## Cross-links
[Related ADRs, phases, research notes]
```

## When to use research vs ADR

| Use research note when... | Use ADR when... |
|---|---|
| Exploring design space | Committing to a specific approach |
| Multiple viable options exist | One option has been chosen |
| Questions outnumber answers | Answers outnumber questions |
| "We might want to..." | "We will..." |
| No implementation timeline | Implementation is next |

## Rules
- Research notes are NOT binding — they capture thinking, not commitments
- Research notes CAN be wrong — that's fine; they're exploratory
- Research notes SHOULD be referenced by the ADR they eventually become
- Research notes SHOULD NOT be deleted when superseded — they capture the design journey
