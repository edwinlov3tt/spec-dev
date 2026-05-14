---
name: push
description: "Safe commit and push — scans for secrets, validates gates, generates conventional commit messages."
arguments:
  - name: message
    description: "Optional commit message override"
    required: false
---

# /push — Safe Commit and Push

Commit and push changes with safety checks. Prevents secrets leakage, validates build gates, and generates good commit messages.

## Steps

### 1. Pre-flight checks

**Secrets scan** — reject if any of these patterns appear in staged changes:
```
- API keys: sk-*, pk_*, AKIA*, ghp_*, gho_*, Bearer *, token = "..."
- Passwords: password = "...", passwd, secret = "..."  
- Connection strings: postgres://*:*@*, mysql://*:*@*, mongodb+srv://
- Private keys: -----BEGIN (RSA|EC|OPENSSH) PRIVATE KEY-----
- .env files with populated values
- AWS credentials, GCP service account JSON
```

If found: **STOP. Do not commit.** Show the user what was found and where. Suggest `.gitignore` or environment variable alternatives.

**Build gate** — run the project's standard gate:
```bash
cargo fmt --check --all
cargo clippy --all-targets --workspace -- -D warnings
cargo test --workspace
```

If any fail: **STOP.** Show the failures. Don't commit broken code.

### 2. Analyze changes

Run `git status` and `git diff --staged` (or `git diff` if nothing staged).

Categorize the changes:
- New files (what they are)
- Modified files (what changed)
- Deleted files (why)

### 3. Generate commit message

Follow conventional commit format:
```
<type>(<scope>): <subject>

<body>

Co-Authored-By: Claude <model> <noreply@anthropic.com>
```

Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`

Rules:
- Subject line < 70 characters
- Body explains WHY, not WHAT (the diff shows what)
- Include `Co-Authored-By` for AI-assisted commits
- If multiple logical changes exist, suggest splitting into multiple commits

### 4. Stage, commit, push

```bash
git add <specific files>  # Not git add -A (too dangerous)
git commit -m "<message>"
git push
```

Show the result to the user.

## Rules
- NEVER use `git add -A` or `git add .` — always stage specific files
- NEVER commit .env files, credentials, or secrets
- NEVER push to main without passing gates (unless user explicitly overrides)
- NEVER skip the secrets scan
- If there are untracked files you're unsure about, ASK before staging
- Prefer multiple focused commits over one giant commit
