---
name: review
description: "Generate a structured review request for an ADR, research note, or implementation — designed for the dual/triple-review pattern."
arguments:
  - name: doc
    description: "Path to the document to review, or 'last-commit' for code review"
    required: true
---

# /review — Dual-Review Pattern

Generate a structured review of a document or implementation. Designed for the dual-review pattern where multiple AI reviewers catch issues before commitments are made.

## For ADR review

Read the ADR and produce:

```
## ADR-NNNN Review

### Decisions assessment
- [OK/CONCERN/MISSING] Decision 1: <assessment>
- [OK/CONCERN/MISSING] Decision 2: <assessment>
...

### Architecture fit
- Does this contradict any accepted ADR? [yes/no, which ones]
- Does this preserve the project's strategic pillars? [list which ones are affected]
- Is the scope bounded? [yes/no, what could creep]

### Implementability
- Can an implementer build this without guessing? [yes/no, what's ambiguous]
- Are acceptance criteria testable? [yes/no, which aren't]
- Are there design questions the ADR leaves open? [list them]

### What the ADR gets right
[Genuine strengths — not just flattery]

### Recommended amendments
1. [Specific amendment with rationale]
2. [...]

### Verdict: Ready to accept | Needs amendments | Needs rethinking
```

## For research note review

Read the note and assess:
- Is the design direction sound?
- What's missing that an ADR would need?
- What's speculative vs proven?
- Are there simpler alternatives?

## For code review (`/review last-commit`)

Review the most recent commit(s):

```
## Code Review: <commit message>

### Changes summary
[What changed and why]

### Correctness
- [OK/CONCERN] <file>: <assessment>
...

### Style and conventions
- Project naming conventions followed? [yes/no]
- Error handling matches project patterns? [yes/no]
- Test coverage adequate? [yes/no]

### Performance
- Any hot-path concerns? [yes/no]
- Unnecessary allocations? [yes/no]

### Missing
- Tests that should exist but don't
- Edge cases not handled
- Documentation not updated

### Verdict: Ship | Fix then ship | Rethink
```

## The dual-review philosophy

Cross-cutting decisions benefit from multiple perspectives:
- **Reviewer 1** (Claude Desktop/GPT) → catches architectural issues, strategic misalignment
- **Reviewer 2** (Claude PM) → catches implementation concerns, codebase-specific issues
- **The synthesis** is stronger than either alone

When sending a doc for external review, format the request:
```
Review this [ADR/research note/implementation].

Key questions:
1. [Specific question you want answered]
2. [Specific concern you want validated]

Context: [1-2 sentences of project context the reviewer needs]
```
