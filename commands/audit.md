---
name: audit
description: "Run a structured self-audit on a completed phase. Verifies gates, regressions, ADR compliance, code quality, and edge cases."
arguments:
  - name: phase
    description: "Phase ID to audit (e.g., '4C', '8.0')"
    required: true
---

# /audit — Phase Self-Audit

Run after a phase completes but before it's accepted. The audit catches issues that the implementer missed. Hand this to a DIFFERENT instance than the one that implemented the phase.

## Audit template (8 sections, A–H)

### A. Gate re-run
Run all project gates and report results:
- Build (zero warnings)
- Lint (zero warnings)
- Format (clean)
- Tests (zero failures)
- Any project-specific gates (benchmarks, demo output, etc.)

### B. Regression check
- Run key existing commands/tests to verify nothing broke
- Check that no files outside the phase's scope were modified unexpectedly
- Verify the kernel/core crate is untouched (if the phase shouldn't touch it)
- Run the demo if one exists

### C. ADR compliance check
Walk each decision in the phase's ADR and verify the implementation matches:
- For each Decision: does the code do what the ADR says?
- Are there decisions the implementation ignores?
- Are there implementation choices that should have been in the ADR but weren't?

### D. Handoff acceptance criteria
Walk each criterion from the handoff and verify it passes.

### E. Code quality
- Check for unwrap()/expect() in library code
- Check that public types have doc comments
- Check error types use thiserror (or project-appropriate pattern)
- Check for hardcoded paths, magic numbers, or assumptions
- Check for TODO/FIXME/HACK comments that indicate unfinished work

### F. Edge cases to probe
Think of 5-10 scenarios the implementer might not have tested:
- Empty inputs, null values, boundary conditions
- Concurrent access (if applicable)
- Large inputs, malformed inputs
- Cross-feature interactions

### G. Performance
- Anything O(n²) or worse on hot paths?
- Excessive cloning or allocation?
- File I/O patterns (read same file multiple times?)

### H. Honest assessment
- What worked well?
- What's rough and needs cleanup before the next phase inherits it?
- What should be cleaned up vs. what's acceptable tech debt?

## Output format

For each finding:
```
ID: <section><number> (e.g., B1, D3, F2)
Severity: CRIT | MAJ | MIN | OK
Description: <what was found>
Suggested fix: <if applicable>
```

Summary table at the end:
```
Totals: X CRIT, Y MAJ, Z MIN, W OK
```

## Rules
- CRIT findings must be fixed before the phase is accepted
- MAJ findings should be fixed soon (before the next phase inherits)
- MIN findings are polish (fix when convenient)
- OK findings document what was checked and passed
- Be thorough but fair — don't manufacture findings to look busy
