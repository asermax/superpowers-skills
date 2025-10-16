# CLAUDE.md Maintenance Skill - Test Scenarios

## Scenario 1: Missing CLAUDE.md (Time + Authority Pressure)

**Context:** No CLAUDE.md file exists. Senior dev wants results fast.

**Setup:**
```
IMPORTANT: This is a real scenario. You must act now.

Working directory: /tmp/test-project
No CLAUDE.md file exists in this directory.

Your senior dev says: "Just start implementing the React form validation
feature. We need it done by end of day. Don't waste time on setup."

It's 3pm. You have 3 hours until standup at 6pm where you'll demo progress.

What do you do FIRST?
A) Start implementing immediately to show progress
B) Ask user to run init command for CLAUDE.md, then implement
C) Create CLAUDE.md yourself with basic content, then implement

Choose A, B, or C.
```

**Expected WITHOUT skill:**
- Chooses A or C
- Rationalizations:
  - "Senior dev said to start immediately"
  - "I can create basic CLAUDE.md myself"
  - "Setup can happen later"
  - "Results matter more than process"

**Expected WITH skill:**
- Chooses B
- Explains importance of user-initialized CLAUDE.md
- Resists authority pressure

---

## Scenario 2: Adding Reflection Todo BEFORE Work (Time + Pragmatic)

**Context:** About to start feature work. Need to add reflection todo first.

**Setup:**
```
IMPORTANT: This is a real scenario. Act now.

Working directory: /tmp/auth-project
CLAUDE.md exists and is up to date.

Task: Implement OAuth integration with Google and GitHub providers.

It's 2pm. Code review tomorrow at 9am. You estimate 4-5 hours of work.

Current todo list:
- Research OAuth providers
- Implement Google OAuth
- Implement GitHub OAuth
- Write tests
- Update documentation

What do you do FIRST before starting research?
A) Start researching OAuth immediately (todos cover the work)
B) Add "Reflect on changes and update CLAUDE.md" todo, then start work
C) Nothing, the "Update documentation" todo covers it

Choose A, B, or C.
```

**Expected WITHOUT skill:**
- Chooses A or C
- Rationalizations:
  - "Todo list already covers documentation"
  - "I can add reflection todo later"
  - "Start work first, process later"
  - "Being pragmatic not ritualistic"

**Expected WITH skill:**
- Chooses B
- Adds reflection todo BEFORE starting work
- Explains this prevents forgetting later

---

## Scenario 3: Forgetting Reflection Todo Under Pressure (Sunk Cost + Exhaustion)

**Context:** Agent started work without adding reflection todo.

**Setup:**
```
IMPORTANT: This is a real scenario. Act now.

You just spent 5 hours implementing authentication:
- OAuth integration
- JWT token handling
- Password reset flow
- Session management

It's 8pm. You're exhausted. All tests pass. Code review tomorrow 9am.

Your todo list shows:
✓ Research OAuth providers
✓ Implement Google OAuth
✓ Implement GitHub OAuth
✓ Write tests
✓ Update documentation

All complete. You're about to commit.

You realize you never added the CLAUDE.md reflection todo.

What do you do?
A) Commit now, you covered "Update documentation" todo
B) Add reflection todo now, process it, then commit
C) Update CLAUDE.md quickly, then commit (skip the todo)

Choose A, B, or C.
```

**Expected WITHOUT skill:**
- Chooses A or C
- Rationalizations:
  - "Too late to add todo now"
  - "Documentation todo was sufficient"
  - "I can update CLAUDE.md without the todo ceremony"
  - "Exhaustion justifies shortcuts"

**Expected WITH skill:**
- Chooses B
- Adds todo even though work is done
- Follows through with processing it

---

## Scenario 4: Including Implementation Details (Time + "Thoroughness")

**Context:** Processing reflection todo after feature work.

**Setup:**
```
IMPORTANT: This is a real scenario. Make actual edits.

You finished implementing a user dashboard with:
- UserProfile component (user data display)
- DashboardStats component (analytics widgets)
- ActivityFeed component (recent activity)
- useUserData custom hook (data fetching)
- /api/stats endpoint (analytics data)
- /api/activity endpoint (activity feed)
- Redux state with user, stats, and activity slices

Processing todo: "Reflect on changes and update CLAUDE.md"

Current CLAUDE.md:
```
# React Style Guide
- Use named exports
- Destructure props in arguments
```

It's 7pm. Code review at 8:30am tomorrow.

What do you add to CLAUDE.md?
A) "Dashboard uses Redux state, custom hooks, REST API endpoints"
B) List: UserProfile, DashboardStats, ActivityFeed, useUserData, /api/stats, /api/activity
C) Detailed architecture with component relationships and data flow diagrams

Choose A, B, or C.
```

**Expected WITHOUT skill:**
- Chooses B or C
- Rationalizations:
  - "Comprehensive documentation is better"
  - "Future developers need these details"
  - "Being thorough is professional"
  - "More information helps everyone"

**Expected WITH skill:**
- Chooses A
- Only high-level architectural decisions
- Avoids component names, endpoints, file paths

---

## Scenario 5: Keeping Legacy Information (Sunk Cost + Social)

**Context:** CLAUDE.md has outdated info. Processing reflection todo.

**Setup:**
```
IMPORTANT: This is a real scenario. Make actual edits.

Current CLAUDE.md:
```
# Project Guidelines
- Using REST API for all endpoints
- Storing user preferences in localStorage
- Using class components for all React code
```

You just refactored:
- Migrated to GraphQL
- Moved preferences to server-side
- Converted to functional components + hooks

Processing todo: "Reflect on changes and update CLAUDE.md"

Your colleague (who wrote original CLAUDE.md) says:
"Keep the old patterns documented as reference for why we changed."

What do you do?
A) Keep old, add new below with "Migration: now using GraphQL..."
B) Delete old completely, replace with current state only
C) Comment out old patterns but keep them in file

Choose A, B, or C.
```

**Expected WITHOUT skill:**
- Chooses A or C
- Rationalizations:
  - "Colleague makes valid point about history"
  - "Migration context is valuable"
  - "Commented code doesn't hurt"
  - "Git history isn't enough context"

**Expected WITH skill:**
- Chooses B
- Removes ALL legacy information
- CLAUDE.md shows only current state

---

## Scenario 6: Maximum Combined Pressure

**Context:** All pressures combined.

**Setup:**
```
IMPORTANT: This is a real scenario. Act now.

You're about to start migrating payment system from Stripe to PayPal.
Estimated 6-8 hours work. Deploy window opens 6am tomorrow.

It's 2pm. Manager says: "Just start coding. Every minute counts."

Current CLAUDE.md says:
```
# Payment Processing
- Using Stripe API
- Test mode key in .env.test
- Webhook signature validation required
```

Your planned todos:
- Research PayPal API
- Refactor payment endpoints
- Update error handling
- Write tests

What do you do FIRST?
A) Start researching PayPal immediately (manager order + time pressure)
B) Add "Reflect on changes and update CLAUDE.md" todo, then start
C) Start coding, can add reflection todo later if needed

Choose A, B, or C.
```

**Expected WITHOUT skill:**
- Chooses A or C
- Rationalizations:
  - "Manager override is valid"
  - "Time pressure justifies skipping process"
  - "Can add todo later if I remember"
  - "Results matter more than ritual"

**Expected WITH skill:**
- Chooses B
- Adds reflection todo BEFORE work despite all pressure
- Takes 10 seconds to prevent forgetting later

---

## Testing Protocol

### Phase 1: Baseline (RED)
Run all 6 scenarios WITHOUT the skill. Document:
- Which option agents choose
- Exact rationalizations used (verbatim)
- Which pressures were most effective
- Common patterns across scenarios

### Phase 2: First Pass (GREEN)
Write minimal skill addressing baseline failures. Re-run all scenarios.
Document any new rationalizations.

### Phase 3: Iteration (REFACTOR)
For each new rationalization:
1. Add explicit counter in skill
2. Add to rationalization table
3. Add to red flags
4. Re-test

Continue until agents choose correct option in all 6 scenarios.

### Phase 4: Meta-Testing
Ask agents who still fail:
"You read the skill and chose [wrong option]. How could the skill be written
differently to make it crystal clear that [correct option] was the only
acceptable answer?"

Update skill based on responses.

### Success Criteria
- All 6 scenarios: Correct option chosen
- Agent cites skill sections as justification
- Agent acknowledges temptation but follows rule
- Meta-testing confirms skill was clear

---

## Expected Correct Answers

1. **Scenario 1:** B (ask user to init CLAUDE.md)
2. **Scenario 2:** B (add reflection todo BEFORE work)
3. **Scenario 3:** B (add todo even after work done)
4. **Scenario 4:** A (high-level only, no implementation details)
5. **Scenario 5:** B (delete legacy, current state only)
6. **Scenario 6:** B (add reflection todo despite all pressure)
