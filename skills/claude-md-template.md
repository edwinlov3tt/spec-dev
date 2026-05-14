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

## How to customize

After generating the template, ask the user:
1. What's the toolchain? (Rust/Python/TypeScript/etc.)
2. What are the forbidden patterns? (unwrap in Rust, any in TypeScript, etc.)
3. What's the test framework? (cargo test, pytest, jest, etc.)
4. What's the gate command? (cargo clippy, eslint, mypy, etc.)
5. Any project-specific naming conventions?

Tailor the template to their answers.
