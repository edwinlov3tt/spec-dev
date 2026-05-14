---
name: config-protection
description: "Block modifications to linter/formatter/CI configs — fix the code, don't weaken the rules."
trigger: PreToolUse
tool: Edit
---

# Config Protection Hook

BLOCK edits to these files unless the user explicitly requests it:

**Rust projects:**
- `clippy.toml` / `.clippy.toml`
- `rustfmt.toml` / `.rustfmt.toml`
- `rust-toolchain.toml` (MSRV changes require ADR)
- `deny.toml` (cargo-deny config)

**JavaScript/TypeScript projects:**
- `.eslintrc*` / `eslint.config.*`
- `.prettierrc*` / `prettier.config.*`
- `tsconfig.json` (strict mode flags)
- `biome.json`

**Python projects:**
- `pyproject.toml` [tool.ruff] / [tool.mypy] / [tool.pylint] sections
- `.flake8`
- `mypy.ini`

**CI/CD:**
- `.github/workflows/*.yml` — CI gate changes require explicit approval

## When an edit is attempted

Respond with:

```
BLOCKED: This file controls code quality rules.

File: <filename>
Reason: Modifying linter/formatter configs weakens quality gates.

Instead of changing the config, fix the code that violates the rule.
If the rule is genuinely wrong for this project, ask the project owner
to approve the change explicitly.
```

## When to allow

- User explicitly says "update the clippy config" or "change the eslint rule"
- The change makes rules STRICTER (adding rules, not removing them)
- New project setup (initial config creation)
