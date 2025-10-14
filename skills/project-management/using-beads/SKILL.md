---
name: Using Beads for Issue Tracking
description: Dependency-aware task management with bd - track all work, model dependencies, use bd ready for next tasks
when_to_use: when discovering new work during implementation, when picking up new tasks, when creating issues for features, or when working in codebases using bd for issue tracking
version: 1.0.0
dependencies: bd (beads CLI tool)
---

# Using Beads for Issue Tracking

## Overview

**Beads (bd) is a dependency-aware issue tracker designed for AI-supervised workflows.** Track all discovered work immediately, model dependencies explicitly, and use `bd ready` to find unblocked work.

**Core principle:** Every piece of work gets tracked as an issue with proper dependencies. No work is too small to track.

## When to Use

Use bd when:
- Discovering new work while implementing a feature
- Finding bugs, missing tests, or documentation issues
- Planning new features that need task breakdown
- Looking for next task to work on
- Working in any codebase that uses bd for tracking

**Critical symptoms that mean you should use bd:**
- "This is too simple to create an issue for"
- "I'll just fix this quickly without tracking"
- "I can add dependencies later"
- "Let me just start coding, I'll document it after"

## Core Workflows

### 1. Discovering Work → Create Issue Immediately

**When you discover ANY work (bugs, missing tests, tech debt, improvements):**

```bash
# Create issue immediately
bd create "Fix inconsistent error handling in auth module"

# If it blocks other work or is blocked by something
bd create "Add tests for auth error paths" -d "Missing test coverage found during payment-1"
bd dep add payment-1 auth-tests-1 --type discovered-from
```

**Rule:** ALL discovered work gets tracked. No exceptions.

### 2. Finding Next Work → Use bd ready

**When you have time to pick up new work:**

```bash
# Find unblocked work ready to claim
bd ready

# NOT this:
bd list  # Wrong - shows all issues, not just ready ones
# NOT this either: just start coding without checking
```

**`bd ready` shows issues with `status='open'` AND no blocking dependencies.**

### 3. Creating Issues for Features → Model Dependencies

**When breaking down a feature into tasks:**

```bash
# Create all issues with proper dependencies
bd create "Update profile data model"
bd create "Create PUT /users/:id endpoint"
bd create "Add validation middleware"
bd create "Add edit button to profile page"
bd create "Write tests for profile editing"

# Model dependencies (what blocks what)
bd dep add api-endpoint-1 data-model-1 --type blocks  # endpoint needs model first
bd dep add validation-1 api-endpoint-1 --type blocks  # validation needed before endpoint complete
bd dep add tests-1 api-endpoint-1 --type related      # tests related but can be parallel
```

**Dependencies are mandatory, not optional metadata.**

## Dependency Types

| Type | When to Use | Example |
|------|-------------|---------|
| `blocks` | Task B must complete before task A | Middleware blocks endpoint implementation |
| `related` | Soft connection, doesn't block | Tests related to feature |
| `parent-child` | Epic/subtask hierarchy | Feature epic with implementation subtasks |
| `discovered-from` | Found during other work | Bug found while implementing feature |

## Quick Reference

| Task | Command | When |
|------|---------|------|
| **Create issue** | `bd create "description"` | Immediately when discovering work |
| **Find next work** | `bd ready` | When choosing what to work on |
| **Add dependency** | `bd dep add issue-1 issue-2 --type blocks` | When creating issues with dependencies |
| **Check dependencies** | `bd dep tree issue-1` | Before starting work on issue |
| **Update status** | `bd update issue-1 --status in_progress` | When starting work |
| **Close issue** | `bd close issue-1` | When work is complete |

## Common Mistakes

### ❌ "Too Simple to Track"

**Wrong:**
```bash
# Found typo in error message, just fix it without creating issue
git commit -m "fix: typo in error message"
```

**Right:**
```bash
# Create issue even for 2-character change
bd create "Fix error message typo in validation.ts"
# Then fix it
git commit -m "fix: typo in error message (closes typo-1)"
```

**Why:** "Too simple" issues accumulate into invisible technical debt. Tracking shows work actually done.

### ❌ "Dependencies Can Wait"

**Wrong:**
```bash
# Create issues quickly, will add dependencies later
bd create "Task 1"
bd create "Task 2"
bd create "Task 3"
# (dependency modeling postponed due to time pressure)
```

**Right:**
```bash
# Model dependencies when creating, even if tight on time
bd create "Task 1"
bd create "Task 2"
bd create "Task 3"
bd dep add task-2 task-1 --type blocks
bd dep add task-3 task-2 --type blocks
# Takes 2 extra minutes, prevents hours of rework
```

**Why:** Dependencies model reality. Wrong order = wasted effort, rework, or blocked progress.

### ❌ Using bd list Instead of bd ready

**Wrong:**
```bash
# Checking what to work on next
bd list
# Reading through all issues manually to find something unblocked
```

**Right:**
```bash
# Finding next work
bd ready
# Shows only unblocked work, ready to claim
```

**Why:** `bd ready` is designed for this. It filters by status and dependencies automatically.

## Red Flags - STOP and Create Issue

- "This is too simple to track"
- "I'll document it after I finish"
- "Just a quick fix, no issue needed"
- "Dependencies are just metadata"
- "I can add dependencies tomorrow"
- "Let me start coding, I'll create issues later"
- "Creating an issue costs more time than the fix"

**All of these mean: Create issue NOW. Model dependencies NOW.**

## Real-World Impact

From using bd in development workflows:
- Discovered work tracked immediately prevents forgotten technical debt
- Dependency modeling eliminates "blocked by surprise" situations
- `bd ready` reduces decision overhead for next-task selection
- Even "30-second fixes" tracked = visible work contribution
