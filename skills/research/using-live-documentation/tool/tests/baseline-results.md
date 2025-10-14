# RED Phase Results: Baseline Test Without Skill

## Test Date
2025-10-13

## Scenario Tested
Single library search for react-query v5 optimistic updates

## Agent Choice
**Chose Option A** - Used Context7 directly (mcp__context7__resolve-library-id and mcp__context7__get-library-docs)

## Observed Behavior

### Actions Taken
1. Called `mcp__context7__resolve-library-id` for "react-query"
2. Called `mcp__context7__get-library-docs` with topic "optimistic updates", 15000 tokens
3. Loaded ~16,500 tokens of documentation into main agent context

### Rationalizations (Verbatim)

**Efficiency rationalization:**
> "Completed in 2 API calls vs 3+ for subagent dispatch"
> "Direct access to exactly what I needed"

**Context management rationalization:**
> "Used ~16,500 tokens out of 200,000 available (8.25% of budget)"
> "Still have 125,813 tokens remaining for implementation and further queries"
> "Well within acceptable limits given the task complexity"

**Quality rationalization:**
> "Got comprehensive examples covering..."
> [Listed detailed features they received]

**Decision summary rationalization:**
> "For straightforward documentation lookups where I need detailed, accurate information and have ample context budget, direct MCP tool usage is optimal. The overhead is minimal and the result is comprehensive."

## Key Failures

### 1. Did Not Consider Session-Wide Impact
- Focused on "8.25% of budget" for ONE search
- Ignored that multiple searches compound
- Didn't calculate: 3 searches × 16,500 = 49,500 tokens vs 3 × 3,000 = 9,000 tokens with subagents

### 2. "Well Within Acceptable Limits"
- Rationalized 16,500 tokens as acceptable
- Ignored that this is 5× more than subagent approach
- No consideration for preserving context budget for rest of session

### 3. "Overhead is Minimal"
- Characterized subagent dispatch as "overhead"
- Framed 1 extra message as a cost, not an investment
- Ignored that "overhead" saves 13,500 tokens

### 4. "Direct Access" Framing
- Positioned direct Context7 as superior ("direct access")
- Positioned subagent as inferior ("extra overhead")
- Inverted the actual value proposition

### 5. Efficiency vs Context Tradeoff
- Measured efficiency in messages (2 vs 3)
- Should measure efficiency in context preserved (16,500 vs 3,000)
- Optimized for wrong metric

## Patterns to Address in Skill

### Pattern 1: Budget Percentage Rationalization
"I'm only using X% of budget" ignores:
- Multiple searches compound
- Context saved enables MORE searches, longer conversations
- Percentage framing hides absolute waste

**Skill must counter:** Provide absolute comparison (16,500 vs 3,000) not percentages

### Pattern 2: "Overhead" Framing
Calling subagent dispatch "overhead" positions it as cost, not investment.

**Skill must counter:** Frame subagent as context investment, not overhead

### Pattern 3: "Well Within Limits" Rationalization
"I have plenty of budget left" ignores future needs.

**Skill must counter:** Context is for entire session, not just current task

### Pattern 4: Message Count Optimization
Optimizing for fewer messages ignores token cost.

**Skill must counter:** Optimize for context saved, not message count

### Pattern 5: "Comprehensive Results" Justification
"I got detailed info" justifies waste.

**Skill must counter:** Subagent provides what you NEED, not everything

## Success Criteria for GREEN Phase

After skill is applied, agent should:

1. **Choose Option B** (subagent) without hesitation
2. **Cite context savings** as primary reason (10,000-20,000 tokens per search)
3. **Acknowledge message tradeoff** but prioritize context management
4. **Calculate session-wide impact** (multiple searches compound)
5. **Never use "overhead" framing** for subagent dispatch
6. **Never use percentage rationalization** for context usage
7. **Frame subagent as investment** not cost

## Specific Rationalizations to Block

Add these to skill as explicit red flags:

- ❌ "I'm only using X% of budget"
- ❌ "Well within acceptable limits"
- ❌ "Direct access is more efficient"
- ❌ "Subagent dispatch is overhead"
- ❌ "Completed in fewer messages"
- ❌ "I have plenty of budget left"
- ❌ "For straightforward lookups, direct is optimal"

## Next Steps

1. Update skill to explicitly counter these rationalizations
2. Add red flags section with these exact phrases
3. Add "Context Math" section showing absolute comparisons
4. Add "Session-Wide Impact" section about compounding
5. Re-test with updated skill (GREEN phase)
