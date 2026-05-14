---
name: post-edit-quality
description: "Run fast quality checks after code edits — catch lint/format/type issues immediately, not at commit time."
trigger: PostToolUse
tool: Edit
---

# Post-Edit Quality Gate

After any code file is edited, run fast quality checks immediately. Catching issues at edit time is 10x cheaper than catching them at commit time.

## What to check (by file type)

**Rust (.rs):**
```bash
cargo clippy --all-targets -p <affected-crate> -- -D warnings 2>&1 | head -20
```
If clippy warnings appear, show them inline: "Clippy warning in the file you just edited: ..."

**React/TypeScript (.tsx, .ts):**
```bash
npx tsc --noEmit --pretty 2>&1 | head -20
npx eslint <file> 2>&1 | head -10
```
Also check for React-specific issues:
- Missing `key` prop in `.map()` renders
- `useEffect` with missing dependencies
- Direct DOM manipulation instead of state
- `console.log` left in component code
- Inline styles that should be in CSS/Tailwind

**JavaScript (.js, .jsx):**
```bash
npx eslint <file> 2>&1 | head -10
```

**Python (.py):**
```bash
ruff check <file> 2>&1 | head -10
```

**CSS/Tailwind (.css, .scss):**
No automated check — but warn if editing global styles that could affect other components.

## What NOT to do

- Don't run the full test suite (too slow for post-edit)
- Don't block the edit (PostToolUse can't block — just warn)
- Don't run formatting (the user may not want auto-format)
- Don't check unrelated files (only the edited file's crate/package)

## Output format

If issues found:
```
Quality check after editing <file>:
  ⚠ <issue description>
  ⚠ <issue description>
```

If clean: say nothing (don't clutter output with "all good" messages).
