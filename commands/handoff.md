---
name: handoff
description: "Generate an implementation handoff document for a phase — the prompt that an implementing instance needs to build the feature."
arguments:
  - name: phase
    description: "Phase ID (e.g., '4C', '8.0', '7A.7')"
    required: true
---

# /handoff — Implementation Handoff Generator

A handoff document is the bridge between "what to build" (ADR) and "how to build it" (implementation). It contains everything a fresh Claude instance needs to implement a phase without reading the full project history.

## What to generate

Read the ADR for the specified phase, then produce a handoff at `docs/handoffs/phase-<id>-handoff.md`:

```markdown
# Phase <ID> Handoff — <Title>

**Status:** Proposed
**Date:** <today>
**ADR:** [ADR-NNNN](link) (Accepted)
**Estimated effort:** <X sessions>
**Crate(s) touched:** <list>

---

## What this phase does
[1-3 sentences. What changes after this ships.]

## Scope (what to build)
[Concrete list of deliverables with enough detail to implement]

## What NOT to build
[Explicit out-of-scope items — prevents scope creep]

## Implementation path
### Step 1: <name>
[What to do, which files, code shapes if helpful]
### Step 2: <name>
[...]

## Files to modify
| File | Change |
|---|---|
| path/to/file | What changes |

## Tests to write
[List specific test scenarios]

## Acceptance criteria
[Numbered, testable — inherited from ADR + implementation-specific]

## Dependencies
[Crates, external deps, version pins]

## Cross-links
[ADR, research notes, related phases]
```

## Rules

1. **Read the ADR first.** The handoff inherits acceptance criteria from the ADR — don't invent different ones.
2. **Be concrete.** "Add the field" is bad. Rust: "Add `pub element_type: Option<String>` to `ParsedDimension` in `schema.rs`." React: "Add `elementType?: string` to the `DimensionProps` interface in `types.ts`."
3. **Include code shapes** when the implementation path isn't obvious. Type signatures, component structures, pseudocode.
4. **List every file** that needs modification. The implementer shouldn't discover files mid-implementation.
5. **Specify the gate** at the bottom. Detect project type:
   - Rust: `cargo fmt --check --all && cargo clippy --all-targets --workspace -- -D warnings && cargo test --workspace`
   - React/TS: `npx tsc --noEmit && npx eslint . --max-warnings 0 && npm test -- --watchAll=false`
   - Python: `ruff check . && mypy . && pytest`

## Also generate: the implementation prompt

After writing the handoff, generate a standalone prompt that can be handed to another Claude instance:

```
You are implementing Phase <ID> — <Title>.

Read these docs in order:
1. <ADR path>
2. <handoff path>
3. <any research notes>

Summary: [2-3 sentences]

Critical constraints:
- [binding rules from ADR]
- [what NOT to change]

[Implementation-specific guidance]

Gate: [project-appropriate gate — see /push for detection rules]
```

Show this prompt to the user so they can copy-paste it to the implementing instance.
