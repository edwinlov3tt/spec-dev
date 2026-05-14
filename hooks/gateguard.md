---
name: gateguard
description: "Force investigation before risky edits — list importers, affected API, verify schemas. Prevents changes made without understanding impact."
trigger: PreToolUse
tool: Edit
---

# GateGuard — Fact-Forcing Before Edits

When Claude is about to edit a file, force it to investigate the impact BEFORE making changes. This prevents the failure mode where an LLM confidently edits a file without understanding who depends on it.

## For Edit/Write operations

Before allowing the edit, require the following information:

**1. Who imports this file?**
Search for imports/uses of the file being edited. List the dependent files.

**2. What public API surface is affected?**
If the edit changes a public function signature, struct field, or type — what callers break?

**3. Do data schemas still match?**
If the edit changes a data structure, verify that serialization/deserialization (YAML, JSON, protobuf) still matches.

**4. What instruction authorizes this change?**
Quote the specific user instruction or ADR decision that motivates this edit. If you can't quote one, STOP and ask.

## For destructive Bash commands

Before allowing: `rm -rf`, `git reset --hard`, `git push --force`, `DROP TABLE`, `DELETE FROM`:

**1. List all targets** — What files/branches/tables will be affected?
**2. Rollback plan** — How do you recover if this is wrong?
**3. Quote authorization** — What specific instruction asked for this?

## When to skip

- Reading files (Read tool) — always allowed
- Creating NEW files (Write to a path that doesn't exist) — allowed (no existing dependents)
- Test files — lower scrutiny (tests don't have importers in the same way)
- Documentation files — lower scrutiny

## Why this works

LLMs will answer "yes" to confirmation prompts even when they haven't investigated. Asking "are you sure?" is useless. Asking "list the files that import this module" forces the actual investigation. The act of looking creates awareness that self-evaluation never did.

Adapted from the GateGuard pattern in everything-claude-code (affaan-m).
