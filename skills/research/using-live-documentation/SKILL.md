---
name: Using Live Documentation
description: Dispatch subagents to fetch library documentation with massive context savings (10,000-20,000 tokens per search)
when_to_use: Any time you need to use, explain, or implement functionality from a library, framework, or technical tool. When you're tempted to load Context7 docs directly.
version: 2.0.0
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

You MUST use documentation search when you encounter ANY of these:

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

### Step 2: Dispatch Documentation Search Subagent

**Why subagent instead of direct Context7:**
- Saves 10,000-20,000 tokens of context in main agent
- Subagent filters docs to only what you need
- Main agent stays focused on implementation
- Better token management across the session

**How to dispatch (template at bottom of this file):**

1. Load the prompt template:
   ```
   ${SUPERPOWERS_SKILLS_ROOT}/skills/research/using-live-documentation/tool/prompts/documentation-search-agent.md
   ```

2. Fill in the template:
   - `{LIBRARY_NAME}`: Package/library name (e.g., "react-query", "fastapi")
   - `{TOPIC}`: Specific concept (e.g., "optimistic updates", "path parameters")
   - `{WHAT_TO_FIND}`: What implementation details you need

3. Dispatch using Task tool:
   ```
   Use: Task
   Parameters:
     - subagent_type: general-purpose
     - description: "Search [library] docs for [topic]"
     - prompt: [filled template]
   ```

**Template filling example:**
```
{LIBRARY_NAME} → react-query
{TOPIC} → optimistic updates
{WHAT_TO_FIND} → API signatures for useMutation with onMutate, how to handle rollback on error, type definitions for context parameter
```

## Red Flags - STOP

If you're thinking ANY of these, you're about to violate the skill:

### Context Rationalization Flags
- ❌ "I'm only using X% of budget" - Percentage hides absolute waste
- ❌ "Well within acceptable limits" - Ignores session-wide compounding
- ❌ "I have plenty of budget left" - Context is for ENTIRE session
- ❌ "This is just one search" - "Just one" becomes "just one more"

### Efficiency Framing Flags
- ❌ "Direct access is more efficient" - You're optimizing for wrong metric
- ❌ "Subagent dispatch is overhead" - It's an investment, not overhead
- ❌ "Completed in fewer messages" - Messages don't matter, tokens do
- ❌ "For straightforward lookups, direct is optimal" - Context math doesn't change

### Quality Justification Flags
- ❌ "I got comprehensive examples" - You don't need comprehensive, you need relevant
- ❌ "I can filter the docs myself" - Filtering doesn't remove docs from context
- ❌ "I need detailed information" - Subagent provides exactly what you need

**The context math:**
- Direct Context7: 15,000-25,000 tokens per search
- Subagent: 2,000-5,000 tokens per search
- Difference: 10,000-20,000 tokens SAVED per search
- 3 searches: 48,000 tokens saved
- That's 48,000 tokens for MORE searches, longer conversations, complex implementations

**Session-wide impact:**
If you use Context7 directly 3 times, you've wasted 48,000 tokens. That's enough for:
- 16 MORE subagent documentation searches, OR
- 4-5 medium-sized implementation files, OR
- Hundreds of turns of conversation

**Never use "I have budget left" to justify waste.**

### Step 3: Implement Using Verified Patterns

**After receiving subagent synthesis:**

1. Cite what you learned: "According to react-query v5 docs (from subagent search)..."
2. Use exact API signatures provided
3. Follow recommended patterns from synthesis
4. Note any differences from what you expected
5. If gaps exist, dispatch another search or use WebSearch

**Never:**
- Mix training data patterns with doc patterns
- Assume API names/signatures
- Skip documentation check "to save time"
- Implement first, verify later
- Load raw Context7 docs yourself (defeats the purpose of subagent)

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

✅ Claude: I see this involves react-query v5. Let me dispatch a subagent to search current documentation.

[Loads template: documentation-search-agent.md]
[Fills: LIBRARY_NAME=react-query, TOPIC=optimistic updates,
 WHAT_TO_FIND=useMutation API with onMutate callback, context types, rollback pattern]

[Dispatches Task with filled template]
[Subagent searches docs, returns ~400 word synthesis with API signatures]

According to react-query v5 docs (from subagent search), here's the implementation using the recommended approach...

[Writes code matching synthesis]
[Uses provided API signatures]
[No uncertainties - everything verified]
[Cites: "The subagent found that in v5, context type must match generic parameter..."]
```

**Successes:**
- Dispatched subagent instead of loading docs directly
- Saved 15,000+ tokens in main agent
- Received focused synthesis with only needed information
- Used current API patterns
- Zero uncertainty
- Explicit about what was verified

### Example 3: Handling Gaps

```
Human: How do I handle WebSocket connections in FastAPI?

✅ Claude: Let me dispatch a subagent to search FastAPI documentation.

[Dispatches with: LIBRARY_NAME=fastapi, TOPIC=websockets,
 WHAT_TO_FIND=WebSocket endpoint setup, connection handling, message patterns]

[Subagent reports gaps: Found basic setup but missing error handling patterns]

The subagent found the basic WebSocket setup pattern but noted gaps in error handling.
Let me dispatch another search focused on that.

[Dispatches second subagent with: TOPIC=websocket error handling]
[Receives complete patterns for both setup and error handling]

According to FastAPI docs, here's how to implement WebSocket endpoints with proper error handling...
```

**Key points:**
- First subagent identified gaps clearly
- Second dispatch targeted the specific gap
- Both searches together cost less context than loading full docs
- Main agent stayed focused on implementation

## When NOT to Use Documentation Search

**Skip documentation search for:**
- Language built-ins (Python dict, JavaScript Array)
- Standard library basics (Python os.path, JavaScript fs)
- Well-known universal concepts (HTTP status codes, REST principles)
- Questions about YOUR codebase (use Read/Grep)

**But DO use documentation search for:**
- Third-party libraries, even familiar ones
- Framework-specific patterns
- Version-specific APIs
- Best practices for tools

**When in doubt: dispatch a subagent.** The cost of a subagent search (2,000-5,000 tokens) is tiny compared to:
- Loading full docs yourself (15,000-25,000 tokens)
- Implementing wrong patterns from training data
- Debugging issues caused by outdated knowledge

## Context Management Strategy

**Why subagents are mandatory:**

**Context savings per search:**
- Direct Context7: 15,000-25,000 tokens per search
- Subagent approach: 2,000-5,000 tokens per search
- Savings: 10,000-20,000 tokens per search

**Across a session:**
- 3 direct searches: ~60,000 tokens
- 3 subagent searches: ~12,000 tokens
- Savings: ~48,000 tokens

**That's 48,000 tokens available for:**
- More codebase files
- Longer conversations
- Additional library searches
- Complex implementations

**The subagent handles token progression internally:**
- Starts with 5000 tokens, focused topic
- Increases to 10000 tokens if needed
- Up to 20000 tokens for broad searches
- Reports gaps if not found

**Your job:** Dispatch with clear WHAT_TO_FIND. Subagent handles the search strategy.

**If subagent reports gaps:**
1. Dispatch another subagent with refined topic
2. Use WebSearch for supplementary info
3. Ask human for clarification

**Never load raw Context7 docs yourself.** That defeats the entire purpose of context management.

## Verification Checklist

Before claiming you've implemented something correctly, verify:

- [ ] Dispatched subagent to fetch current documentation
- [ ] Provided clear WHAT_TO_FIND in dispatch
- [ ] Received synthesis with API signatures
- [ ] API signatures match documentation exactly
- [ ] Patterns follow current best practices from synthesis
- [ ] No uncertainties remain about correct approach
- [ ] Can cite documentation source for key decisions
- [ ] Did NOT load raw Context7 docs yourself

**If you have ANY uncertainty after receiving synthesis:**
- Dispatch another subagent with refined topic
- Use WebSearch for supplementary info
- Ask human for clarification

**Never:**
- Load Context7 docs directly in main agent
- Ship uncertain implementation
- Skip documentation search to "save time"

## Common Mistakes

### Mistake 1: "I remember this API"

```
❌ "I know react-query uses useQuery, let me write this..."
✅ "Let me dispatch a subagent to verify the current useQuery API..."
```

**Why it fails:** APIs change. Your memory is from training cutoff.

### Mistake 2: "Subagent overhead isn't worth it"

```
❌ "This is just one search, I'll load Context7 directly..."
✅ "Even one search saves 15,000 tokens. Always dispatch subagent."
```

**Why it fails:** "Just one" becomes "just one more" throughout the session. Context compounds.

### Mistake 3: "I'll verify after writing"

```
❌ [Writes full implementation] "Let me check if this is right..."
✅ [Dispatches subagent first] "Now I'll implement using verified patterns..."
```

**Why it fails:** Fixing wrong code takes longer than writing correct code once.

### Mistake 4: "This is urgent, skip the subagent"

```
❌ [Production down] "No time for subagent, I'll load docs directly..."
✅ [Production down] "Dispatch subagent - urgency doesn't justify wasting 15,000 tokens."
```

**Why it fails:** Urgency doesn't change context math. Subagent dispatch takes 30 seconds, saves 15,000 tokens for the REST of the session.

### Mistake 5: "I can filter docs efficiently myself"

```
❌ "I'll load the full docs and extract what I need..."
✅ "That's what the subagent does, in isolated context. Never load raw docs."
```

**Why it fails:** Even if you filter mentally, the docs are in your context. That's 15,000-25,000 tokens gone permanently.

## Integration with Other Workflows

**With Test-Driven Development:**
1. Dispatch subagent for docs BEFORE writing test
2. Receive synthesis with API signatures
3. Write test using documented patterns
4. Implement using same synthesis

**With Brainstorming:**
1. During design discussion, dispatch subagent for relevant docs
2. Base design on current capabilities from synthesis
3. Don't propose deprecated patterns
4. Verify feasibility with current API

**With Debugging:**
1. Dispatch subagent when error involves library
2. Check if API usage matches synthesis patterns
3. Verify you're using correct version's API
4. Look for migration guides if version changed

## Summary

**Before implementing ANYTHING involving a library/framework:**

1. Recognize trigger (library name → stop)
2. Dispatch subagent with documentation search template
3. Fill template: LIBRARY_NAME, TOPIC, WHAT_TO_FIND
4. Receive synthesis (~400 words + API signatures)
5. Implement using verified patterns from synthesis
6. Cite documentation source

**Critical rules:**
- **NEVER load raw Context7 docs in main agent**
- **ALWAYS dispatch subagent for documentation**
- **Context savings: 10,000-20,000 tokens per search**
- **Your training data is always outdated**
- **Current documentation is always more accurate**
- **Dispatch subagent first, write code second**

**This is not optional. This is mandatory.**
