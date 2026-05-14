---
name: claude-md-template
description: "Generate a CLAUDE.md operating manual for a new project — the binding rules file that governs all Claude instances working on the project."
---

# CLAUDE.md Template Generator

When the user asks to set up a new project for spec-driven development, generate a `CLAUDE.md` at the project root. This is the operating manual — every Claude instance reads it first.

## Template structure

```markdown
# CLAUDE.md — [Project Name]

> **Read this entire file at the start of every session before touching code.**

---

## 0. Hierarchy of authority

1. Specification documents (specs/, ADRs)
2. This file (CLAUDE.md)
3. Any prior chat instructions
4. Your own intuition

If your intuition disagrees with anything above it, your intuition is wrong.

---

## 1. Project identity

| Thing | Value |
|---|---|
| Workspace | [description] |
| Toolchain | [language, version, pinned?] |
| Allowed deps | [list] |
| Banned deps | [list] |

---

## 2. Known weaknesses (and countermeasures)

[Project-specific failure modes. Examples:]
- Renaming for "idiomatic" style when the spec uses exact names
- Adding dependencies not in the allowed list
- Skipping tests before declaring done

---

## 3. Strict requirements — Syntax

### Forbidden patterns
| Pattern | Why | What to do instead |
|---|---|---|

### Required patterns
| Pattern | Where required |
|---|---|

---

## 4. Testing requirements

- Test names from the spec are contracts — don't rename
- Float assertions use epsilon (never ==)
- Tests must be deterministic across runs

---

## 5. The self-check protocol

Run before declaring ANY task done:
1. Format check
2. Lint clean
3. Build clean
4. Tests pass
5. Forbidden-pattern grep shows zero matches

---

## 6. Communication protocol

When asking for clarification:
SPEC QUESTION: [one-line summary]
Context: [where this came up]
What I would do without confirmation: [conservative path]

When reporting completion:
DONE: [task name]
Build/Format/Lint/Tests: [status]
Files: [list]

---

## 7. Definition of Done

- [ ] Code compiles clean
- [ ] Lint clean
- [ ] Formatted
- [ ] All tests pass
- [ ] Forbidden patterns: zero matches
- [ ] Public items documented
- [ ] No new dependency introduced without approval
```

## Language-specific defaults

When the user specifies their stack, pre-populate with these:

### Rust projects
```
Forbidden: .unwrap() in lib code, unsafe without ADR, println! in lib, serde in kernel
Gate: cargo fmt --check --all && cargo clippy --all-targets -- -D warnings && cargo test --workspace
Tests: cargo test, criterion for benchmarks
Naming: snake_case functions, PascalCase types, SCREAMING_SNAKE constants
```

### React/TypeScript projects
```
Forbidden: any type, console.log in components, inline styles (use Tailwind/CSS modules), direct DOM manipulation
Gate: npx tsc --noEmit && npx eslint . --max-warnings 0 && npm test -- --watchAll=false
Tests: jest/vitest for unit, playwright/cypress for E2E
Naming: PascalCase components, camelCase functions/hooks, SCREAMING_SNAKE constants
Extra: useEffect must declare dependencies, keys in .map() renders, no prop drilling beyond 2 levels
```

### Python projects
```
Forbidden: bare except, print() in library code, global mutable state, star imports
Gate: ruff check . && mypy . && pytest
Tests: pytest, hypothesis for property tests
Naming: snake_case functions, PascalCase classes, SCREAMING_SNAKE constants
```

## How to customize

After generating the template, ask the user:
1. What's the toolchain? (Rust/Python/TypeScript/etc.)
2. What are the forbidden patterns specific to their project?
3. What's the test framework?
4. What's the gate command?
5. Any project-specific naming conventions?

Tailor the template to their answers. Use the language defaults above as a starting point.
