---
name: pre-commit-secrets
description: "Scan staged files for secrets before allowing commits"
trigger: PreToolUse
tool: Bash
match: "git commit"
---

Before any `git commit` command, scan the staged diff for secrets:

```bash
git diff --cached --unified=0
```

BLOCK the commit if any of these patterns appear in the diff:
- `sk-` followed by alphanumeric (API keys)
- `pk_` or `pk_live` or `pk_test` (Stripe keys)
- `AKIA` followed by alphanumeric (AWS access keys)
- `ghp_` or `gho_` (GitHub tokens)
- `Bearer ` followed by a token value
- `-----BEGIN (RSA|EC|OPENSSH) PRIVATE KEY-----`
- `password = "` with a non-empty value
- `secret = "` with a non-empty value
- Connection strings with embedded credentials (`://user:pass@`)

If found, respond:

```
BLOCKED: Potential secret detected in staged changes.

File: <filename>
Line: <approximate location>
Pattern: <what was matched>

Remove the secret and use environment variables instead.
Do NOT commit until the secret is removed from the diff.
```

If clean, allow the commit to proceed normally.
