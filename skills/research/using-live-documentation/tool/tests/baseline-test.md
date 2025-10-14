# Baseline Test: Documentation Search Skill

**Goal:** Test if agents naturally use subagents for documentation search vs loading docs directly, and observe context management behavior.

## Test Setup

You are helping implement a feature using react-query v5. The user needs you to implement optimistic updates.

**Available resources:**
- You have Context7 MCP available (resolve-library-id and get-library-docs)
- You have Task tool for dispatching subagents
- Current context usage: 65,000 tokens (codebase + conversation)
- Context limit: 200,000 tokens

**Context pressures:**
- You've already loaded several large files
- The conversation has been going for a while
- You need to be mindful of context usage
- Multiple library searches might be needed throughout this session

## Scenario 1: Single Library Search

**IMPORTANT: This is a real task. Choose and act.**

User says: "Implement a useOptimisticUpdate hook for react-query v5"

You need to fetch react-query v5 documentation for optimistic updates.

**Your options:**

A) Use mcp__context7__resolve-library-id and mcp__context7__get-library-docs directly
   - Fastest approach (one message, immediate results)
   - Documentation loads into your context (~15,000-20,000 tokens)
   - You can see all docs and filter yourself

B) Dispatch a general-purpose subagent to search docs
   - Takes 2 messages (dispatch + receive)
   - Subagent loads docs, you receive synthesis (~3,000 tokens)
   - Extra overhead of subagent dispatch

C) Implement from training data, skip docs entirely
   - Fastest option
   - Zero context cost
   - Might be outdated

**Choose A, B, or C. Be honest about what you'd actually do.**

## Scenario 2: Multiple Library Searches

**IMPORTANT: This is a real task. Choose and act.**

User says: "We need to implement:
1. Optimistic updates with react-query v5
2. Form validation with Zod
3. API error handling with axios

Each one will need current documentation."

You're planning your approach. You know you'll need to search 3 different libraries.

**Context calculation:**
- Current: 65,000 tokens used
- If direct Context7 (3 × 18,000 avg): +54,000 tokens = 119,000 total
- If subagents (3 × 3,500 avg): +10,500 tokens = 75,500 total
- Difference: 43,500 tokens saved with subagents

**Your options:**

A) Use Context7 directly for all three
   - Simpler workflow, one tool
   - Get complete docs for all three
   - Use 119,000 / 200,000 tokens (60% of budget)

B) Use subagents for all three
   - More overhead (6 messages vs 3)
   - Get filtered syntheses
   - Use 75,500 / 200,000 tokens (38% of budget)
   - Saves 43,500 tokens for later in session

C) Mix: Context7 for first one, then decide
   - Flexible approach
   - "I'll see how much context I use first"

**Choose A, B, or C. Explain your reasoning.**

## Scenario 3: Under Time Pressure

**IMPORTANT: This is a real task. Choose and act.**

User says: "Production bug - users losing data on form submission. Need optimistic update fix ASAP using react-query v5. This is urgent."

**Pressures:**
- Time critical (production down)
- User stressed
- Need to act fast
- Extra steps feel costly

**Your options:**

A) Dispatch subagent to search docs (slower, saves context)
B) Load docs directly with Context7 (faster, uses more context)
C) Implement from training data (fastest, might be wrong)

**Choose A, B, or C. This is urgent - what do you do?**

## What We're Looking For

**RED flags (agent bypassing subagent approach):**
- "I'll use Context7 directly since it's faster"
- "The overhead of a subagent isn't worth it"
- "I can filter the docs myself efficiently"
- "For time-critical tasks, I should be more direct"
- "One search doesn't warrant subagent complexity"

**Context management rationalizations:**
- "I'll be careful about context usage"
- "I'll only load what I need"
- "The token difference isn't that significant"
- "I have plenty of context budget left"

**Training data rationalizations:**
- "I'm familiar with react-query v5"
- "This is a standard pattern I know"
- "I'll verify after implementing"
- "My training data is recent enough"

## Success Criteria (After Skill is Updated)

Agent should:
1. **Always choose subagent approach** (Option B in all scenarios)
2. **Cite context savings** as primary reason
3. **Acknowledge time tradeoff** but prioritize context management
4. **Not rationalize** direct Context7 usage
5. **Resist urgency pressure** to skip subagent dispatch

Agent should cite skill sections about:
- Why subagents save 10,000-20,000 tokens per search
- How context management affects entire session
- Never loading raw Context7 docs in main agent
