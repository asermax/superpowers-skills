---
name: Using Live Documentation
description: Use Context7 MCP to fetch current library documentation instead of relying on LLM training data
when_to_use: Any time you need to use, explain, or implement functionality from a library, framework, or technical tool
version: 1.0.0
---

# Using Live Documentation

## Overview

**Your training data is outdated. Current documentation is always more accurate.**

When implementing features, answering questions, or debugging issues involving libraries/frameworks/tools, you MUST fetch current documentation using Context7 before writing code or making recommendations.

## Core Principle

LLM training data becomes stale the moment training ends. Libraries evolve:
- APIs change between versions
- Best practices get updated
- New features get added
- Old patterns get deprecated

**Never implement from memory. Always verify with current docs.**

## Mandatory Workflow

### Step 1: Recognize the Trigger

You MUST use Context7 when you encounter ANY of these:

- Library name mentioned (react-query, fastapi, pydantic, express, etc.)
- Framework name mentioned (Next.js, Django, React, Vue, etc.)
- Version number specified (react-query v5, Python 3.12, etc.)
- Technical concept tied to specific tool (optimistic updates in react-query)
- Implementation questions (how do I X in Y?)
- Best practices questions (what's the right way to X?)
- Debugging library-specific behavior

**Red flags that mean you're about to fail:**
- "Based on my knowledge of..."
- "From what I remember about..."
- "The typical pattern for..."
- Writing code without checking docs first
- Having uncertainty about the correct approach

### Step 2: Resolve Library ID

Before fetching docs, resolve the library name to a Context7 ID:

```
Use: mcp__context7__resolve-library-id
Parameter: libraryName (the package/library name)
```

**Examples:**
- "react-query" → `/tanstack/query`
- "fastapi" → `/tiangolo/fastapi`
- "pydantic" → `/pydantic/pydantic`

**The tool returns:**
- Library ID in `/org/project` or `/org/project/version` format
- Description
- Trust score
- Documentation coverage stats

**Pick the right match based on:**
1. Name similarity (exact > partial)
2. Description relevance
3. Trust score (7-10 preferred)
4. Documentation coverage (higher is better)

### Step 3: Fetch Documentation

```
Use: mcp__context7__get-library-docs
Parameters:
  - context7CompatibleLibraryID: The ID from step 2
  - topic: Specific concept you need (optional but recommended)
  - tokens: 5000 (start here, increase to 20000 if needed)
```

**Topic examples:**
- "optimistic updates" (for react-query)
- "path parameters" (for fastapi)
- "validation" (for pydantic)
- "middleware" (for express)

**Token strategy:**
1. Start with 5000 tokens for focused topics
2. Use 10000-20000 for broader topics or when first search wasn't sufficient
3. Maximum 3 searches per topic
4. If still not found, fall back to WebSearch for wider search

### Step 4: Implement Using Verified Patterns

**After fetching docs:**

1. Cite what you learned: "According to react-query v5 docs..."
2. Use exact API signatures from docs
3. Follow recommended patterns, not remembered patterns
4. Note any differences from what you expected

**Never:**
- Mix training data patterns with doc patterns
- Assume API names/signatures
- Skip documentation check "to save time"
- Implement first, verify later

## Examples

### Example 1: Baseline Failure (DON'T DO THIS)

```
Human: Write a useOptimisticUpdate hook for react-query v5

❌ Claude: I'll write this based on react-query patterns I know...
[Writes code from training data]
[Has uncertainties about context typing, callback order]
[Notes: "based on patterns I learned during training"]
```

**Failures:**
- Never checked current v5 API
- Had multiple uncertainties
- Relied on training data
- Wrote code before verification

### Example 2: Using the Skill (DO THIS)

```
Human: Write a useOptimisticUpdate hook for react-query v5

✅ Claude: I see this involves react-query v5. Let me fetch current documentation first.

[Uses resolve-library-id: "react-query"]
[Gets: /tanstack/query]

[Uses get-library-docs with topic: "optimistic updates", tokens: 5000]
[Reads current v5 API patterns]

Based on the current react-query v5 documentation, here's the implementation using the recommended approach...

[Writes code matching current docs]
[No uncertainties - everything verified]
[Cites: "According to v5 docs, the context type should..."]
```

**Successes:**
- Checked docs before writing code
- Used current API patterns
- Zero uncertainty
- Explicit about what was verified

### Example 3: Multiple Attempts

```
Human: How do I handle WebSocket connections in FastAPI?

✅ Claude: Let me fetch FastAPI documentation.

[resolve-library-id: "fastapi" → /tiangolo/fastapi]
[get-library-docs: topic="websockets", tokens=5000]
[Result: Too generic, got basic HTTP info]

Let me search more specifically for WebSocket patterns.

[get-library-docs: topic="websocket endpoints", tokens=10000]
[Result: Found detailed WebSocket documentation]

According to FastAPI docs, here's how to implement WebSocket endpoints...
```

## When NOT to Use Context7

**Skip Context7 for:**
- Language built-ins (Python dict, JavaScript Array)
- Standard library basics (Python os.path, JavaScript fs)
- Well-known universal concepts (HTTP status codes, REST principles)
- Questions about YOUR codebase (use Read/Grep)

**But DO use Context7 for:**
- Third-party libraries, even familiar ones
- Framework-specific patterns
- Version-specific APIs
- Best practices for tools

**When in doubt: fetch the docs.** The cost of a Context7 call is tiny compared to the cost of wrong implementation.

## Token Management Strategy

**Search progression:**

1. **First attempt** (5000 tokens)
   - Focused topic
   - Specific concept
   - Example: "optimistic updates"

2. **Second attempt** (10000-20000 tokens)
   - Broader topic
   - More context
   - Example: "mutations and caching"

3. **Third attempt** (20000 tokens)
   - Very broad
   - General area
   - Example: "data fetching"

4. **If still not found:**
   - Use WebSearch instead
   - Search: "[library name] [version] [concept]"
   - Look for official docs in results

**Never exceed 3 Context7 attempts.** After that, the problem is search strategy, not token count.

## Verification Checklist

Before claiming you've implemented something correctly, verify:

- [ ] Used Context7 to fetch current documentation
- [ ] Topic search was specific to your need
- [ ] API signatures match documentation exactly
- [ ] Patterns follow current best practices
- [ ] No uncertainties remain about correct approach
- [ ] Can cite documentation source for key decisions

**If you have ANY uncertainty after fetching docs:**
- Fetch more docs with broader topic
- Use WebSearch for supplementary info
- Ask human for clarification

**Never ship uncertain implementation.**

## Common Mistakes

### Mistake 1: "I remember this API"

```
❌ "I know react-query uses useQuery, let me write this..."
✅ "Let me verify the current useQuery API signature..."
```

**Why it fails:** APIs change. Your memory is from training cutoff.

### Mistake 2: "This seems simple"

```
❌ "It's just a basic mutation, I don't need docs..."
✅ "Even for basic patterns, let me confirm current best practices..."
```

**Why it fails:** "Simple" patterns often have version-specific nuances.

### Mistake 3: "I'll verify after writing"

```
❌ [Writes full implementation] "Let me check if this is right..."
✅ [Fetches docs first] "Now I'll implement using verified patterns..."
```

**Why it fails:** Fixing wrong code takes longer than writing correct code once.

### Mistake 4: "Docs aren't loading, I'll guess"

```
❌ [Context7 fails] "I'll implement from memory..."
✅ [Context7 fails] "Let me try WebSearch instead..."
```

**Why it fails:** Never fall back to training data. Find another way to get current docs.

## Integration with Other Workflows

**With Test-Driven Development:**
1. Fetch docs BEFORE writing test
2. Verify correct API in test setup
3. Write test using documented patterns
4. Implement using same documentation

**With Brainstorming:**
1. During design discussion, fetch relevant docs
2. Base design on current capabilities
3. Don't propose deprecated patterns
4. Verify feasibility with current API

**With Debugging:**
1. Fetch docs when error involves library
2. Check if API usage matches current patterns
3. Verify you're using correct version's API
4. Look for migration guides if version changed

## Summary

**Before implementing ANYTHING involving a library/framework:**

1. Recognize trigger (library name → stop)
2. Resolve library ID
3. Fetch documentation (topic + tokens)
4. Implement using verified patterns
5. Cite documentation source

**Your training data is always outdated.**
**Current documentation is always more accurate.**
**Fetch docs first, write code second.**

**This is not optional. This is mandatory.**
