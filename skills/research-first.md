---
name: research-first
description: "Search before coding — check GitHub, package registries, and docs for existing implementations before writing new code. Prevents wheel-reinvention."
---

# Research First

Before implementing ANY significant feature, search for existing solutions. This prevents building something that already exists as a well-tested library or pattern.

## When to trigger

Use this skill when:
- Starting a new feature or module
- Implementing a common pattern (auth, caching, scheduling, etc.)
- Adding a new dependency
- Solving a problem that feels like "someone must have solved this before"

## The research sequence

### Step 1: GitHub code search
```bash
gh search repos "<feature keyword>" --language=<project-language> --sort=stars
gh search code "<specific API or pattern>" --language=<project-language>
```

Look for:
- Libraries that solve this problem directly
- Reference implementations you can learn from
- Common patterns across popular projects

### Step 2: Package registry search
```bash
# Rust
cargo search <keyword>

# Node
npm search <keyword>

# Python
pip search <keyword>  # or search pypi.org
```

Evaluate: stars, downloads, last update, dependency count, MSRV/version compat.

### Step 3: Documentation lookup
Check the official docs for frameworks/libraries already in the project. The feature you're about to build might already exist as a built-in.

### Step 4: Decide

| Finding | Action |
|---|---|
| Well-maintained library exists | Use it (check license + dep tree) |
| Reference implementation exists | Study it, adapt the pattern |
| Multiple approaches exist | Compare in a research note, then pick |
| Nothing exists | Proceed with custom implementation — you've earned it |

## Rules

- Don't spend more than 10 minutes researching. If nothing obvious surfaces, proceed.
- Don't adopt a library with more dependencies than your feature needs
- Don't blindly copy code — understand it first
- DO document what you found (even if you don't use it — saves the next person)
- DO check MSRV/version compatibility before committing to a dependency
