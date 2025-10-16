---
name: Self-Maintaining CLAUDE.md
description: Keep instruction file current with high-level project state, prompt for init, add reflection todo before work
when_to_use: when starting any task in a project, when CLAUDE.md doesn't exist, when completing work that changes project patterns
version: 1.0.0
---

# Self-Maintaining CLAUDE.md

## Overview

**CLAUDE.md is your project's instruction file for Claude.** It must stay current with the project's high-level state.

**Core principle:** Add reflection todo BEFORE starting work, or you WILL forget to update CLAUDE.md later.

## Mandatory Workflow

### 1. Check CLAUDE.md Exists

Before ANY task, check if CLAUDE.md exists in the project root.

**If missing:**
- STOP
- Prompt user: "No CLAUDE.md found. Please run the init command to create it."
- DO NOT create it yourself
- DO NOT continue without it

**Why:** User-initialized CLAUDE.md ensures proper setup and user preferences.

### 2. Add Reflection Todo BEFORE Work

**Before starting ANY implementation task, add this todo:**

```
"Reflect on changes and update CLAUDE.md"
```

**ALWAYS add this todo FIRST, before researching, coding, or planning.**

**No exceptions:**
- Not "later if needed"
- Not "existing documentation todo covers it"
- Not "unnecessary overhead"
- Not "after I see what changes"

**Why:** "Later" means "forgotten". Adding the todo takes 10 seconds. Forgetting to update CLAUDE.md wastes hours of future work.

### 3. Process Reflection Todo

When processing the reflection todo, update CLAUDE.md with:

**✅ DO include:**
- High-level architectural decisions (GraphQL vs REST, Redux vs Context)
- Project-wide patterns (authentication approach, error handling strategy)
- Technology choices (frameworks, major libraries)
- Coding style preferences specific to this project

**❌ DO NOT include:**
- Component names (UserProfile, DashboardStats)
- File paths (src/components/auth/Login.tsx)
- Function names (useUserData, fetchStats)
- API endpoint paths (/api/users, /api/stats)
- Implementation details (3 reducers, 5 components)

**Why:** CLAUDE.md guides HOW to build, not WHAT was built. Implementation details belong in README, not instructions.

### 4. Remove Legacy Information

CLAUDE.md shows ONLY current state.

**When updating:**
- DELETE all outdated information completely
- NO "Migration:" sections
- NO commented-out old patterns
- NO "Previously we used..." references

**Why:** CLAUDE.md is active instructions, not history. Git preserves history. Mixed old/new creates confusion.

## Quick Reference

| Situation | Action |
|-----------|--------|
| No CLAUDE.md exists | Prompt user to run init command |
| Starting new task | Add reflection todo BEFORE work |
| "Update docs" todo exists | ALSO add specific reflection todo |
| Updating CLAUDE.md | Only high-level patterns, delete legacy |
| Manager says skip process | Add reflection todo anyway (10 seconds) |

## Common Mistakes

### ❌ "I'll create CLAUDE.md myself to save time"
Creating it yourself skips user preferences and proper setup.
✅ Fix: Always prompt user to run init command.

### ❌ "Existing documentation todo covers it"
Generic todos get skipped or misinterpreted.
✅ Fix: Add specific "Reflect on changes and update CLAUDE.md" todo.

### ❌ "Adding the todo is unnecessary overhead"
Skipping 10 seconds now costs hours later when CLAUDE.md is outdated.
✅ Fix: Add the todo BEFORE work. Non-negotiable.

### ❌ "I can add the reflection todo later if needed"
"Later" means "forgotten". Always forgotten.
✅ Fix: Add it FIRST, before research, planning, or coding.

### ❌ "Documentation can wait until after implementation"
Waiting means forgetting what decisions were made and why.
✅ Fix: Add reflection todo now, process after implementation.

### ❌ "List all components and endpoints for completeness"
Implementation catalog doesn't help future work.
✅ Fix: Only include high-level architectural decisions.

### ❌ "Keep old patterns as reference for context"
Mixed old/new instructions create confusion and errors.
✅ Fix: Delete all legacy info. Git preserves history.

## Red Flags - STOP

If you're thinking:
- "I can create CLAUDE.md myself"
- "Documentation todo already exists"
- "Adding this todo is ceremony/overhead"
- "I'll add it later if needed"
- "Let me list all the components I built"
- "Keep old patterns for historical context"

**All of these mean: You're violating the workflow. Stop and follow the skill.**

## Why This Matters

**Without reflection todo:**
- You forget to update CLAUDE.md (always)
- Future work uses outdated instructions
- Hours wasted on wrong patterns

**With reflection todo added BEFORE work:**
- You remember to update CLAUDE.md
- CLAUDE.md stays current
- Future work follows correct patterns

The 10 seconds to add the todo saves hours of future confusion.
