---
name: sync
description: "Extract decisions, issues, and state changes from the current conversation and persist them to project docs."
arguments:
  - name: scope
    description: "all | decisions | issues | state"
    required: false
---

# /sync — Conversation Knowledge Extraction

Reviews the current conversation and extracts structured information into persistent project docs. This is how conversation context survives session boundaries.

## What to extract

### Decisions → `docs/decisions/` or amendment to existing ADR
Signals: "decided to", "the approach is", "we'll use X instead of Y", "rejected because", "accepted ADR"
- If it's a new architectural decision → draft an ADR (use `/adr create`)
- If it amends an existing ADR → add an acceptance amendment section
- If it's a minor choice (not ADR-worthy) → add to process-notes or CLAUDE.md

### Issues found → existing issue tracking or research notes
Signals: "bug", "N/A", "broken", "doesn't work", "fails when", "regression"
- If it's a known issue → check if already documented
- If new → add to relevant docs or create a research note
- Include: what broke, where, severity, workaround if any

### State changes → MASTER_PHASE_PLAN.md, CURRENT_STATE.md
Signals: "shipped", "complete", "merged", "accepted", "in progress"
- Phase completions → update master plan (use `/phase complete`)
- ADR acceptances → update decisions index
- New phases proposed → update master plan (use `/phase propose`)

### Memory-worthy items → `.claude/` memory system (if configured)
Signals: user preferences, feedback corrections, project context
- User corrected an approach → save as feedback memory
- Learned something about the project → save as project memory
- Useful external reference → save as reference memory

## Process

1. Scan the conversation from the beginning
2. For each extraction candidate:
   - Check if it's already documented (don't duplicate)
   - Determine the right destination file
   - Draft the content
3. Show the user a summary of what you'll extract:
   ```
   Sync found:
   - 1 ADR acceptance (ADR-0029)
   - 2 phase completions (4C, 7A.7)
   - 1 new research note (ConnectRPC evaluation)
   - 3 master plan updates
   ```
4. Ask for approval before writing files
5. Write the files and commit

## Rules
- Never extract speculative discussion as a decision (only extract what was actually decided)
- Don't create ADRs for trivial choices (use process-notes for small conventions)
- Don't duplicate — check existing docs first
- Show the user what you're about to write before writing it
- Commit the sync as a single commit: `"docs: sync session <date> — <summary>"`
