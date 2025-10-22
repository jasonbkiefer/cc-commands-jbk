# Command: feature-touchpoint-review

Perform a comprehensive, systematic review of all feature touchpoints across frontend, backend, database, MCP servers, and data pipelines. Map all user flows, identify break points, and validate implementation quality.

## Usage
/project:feature-touchpoint-review [feature-name]

Example: `/project:feature-touchpoint-review csat-survey`

if no feature mentioned refer to the branch name or recent changes.

## Implementation

This command will:
1. **Map all user flows** - Document every way users interact with the feature
2. **Identify touchpoints** - List frontend components, backend routes, database tables, MCP servers, data pipelines
3. **Find break points** - Identify potential failure points in each flow
4. **Systematic code review** - Use Grep to examine each touchpoint individually
5. **Test scenarios** - Consider edge cases, empty states, error conditions
6. **Render decision** - Final GO/NO-GO with specific issues found

## Review Phases

### Phase 1: Feature Discovery & Flow Mapping

**Step 1A: Identify feature scope**
- Determine if feature is internal-only or customer-facing
- Identify all user roles that interact with feature
- List all entry points (URLs, buttons, API endpoints)

**Step 1B: Map user flows**
For EACH way users interact with the feature, document:
- User path: Step-by-step user actions
- Touch points: Code locations triggered by each step
- Expected behavior: What should happen
- Potential break points: Where it could fail

Example flows to consider:
- **Display flow**: How does the feature appear to users?
- **Eligibility/access flow**: Who can see/use it? How is access controlled?
- **Interaction flow**: What happens when users interact?
- **Submission/save flow**: How is data processed and saved?
- **Retrieval flow**: How is existing data fetched and displayed?
- **Edit flow**: Can users modify existing data?
- **Delete/archive flow**: Can data be removed?
- **Error flow**: What happens when things go wrong?

### Phase 2: Touchpoint Identification

**Frontend Touchpoints:**
- Components that render feature UI
- State management (Redux, context, local state)
- API calls and data fetching
- Form validation and user input handling
- Error display and loading states
- Conditional rendering logic

**Backend Touchpoints:**
- Route handlers (GET, POST, PUT, DELETE)
- Middleware (auth, validation, error handling)
- Service layer functions
- Database helper functions
- External API calls
- Webhook triggers

**Database Touchpoints:**
- Tables involved (primary and related)
- Queries (SELECT, INSERT, UPDATE, DELETE)
- Joins and relationships
- Indexes used
- Transactions and locking
- Data validation constraints

**MCP Server Touchpoints (if applicable):**
- MCP tool endpoints
- Data transformation logic
- Input validation
- Output formatting
- Error handling

**Data Pipeline Touchpoints:**
- Input data sources
- Transformation steps
- Validation rules
- Output destinations
- Error logging

### Phase 3: Systematic Code Review

For each touchpoint identified in Phase 2:

**Step 3A: Read the code**
Use Read tool to examine full implementation

**Step 3B: Search for patterns**
Use Grep to find:
- Error handling patterns
- Validation logic
- Edge case handling
- SQL queries
- Type definitions
- Test coverage

**Step 3C: Check for common issues**
- ❌ Missing error handling
- ❌ Unvalidated user input
- ❌ SQL injection vulnerabilities
- ❌ Missing null/undefined checks
- ❌ Empty array handling
- ❌ Race conditions
- ❌ Memory leaks
- ❌ Type mismatches
- ❌ Missing authorization checks
- ❌ Hardcoded values that should be configurable

**Step 3D: Validate data flow**
- Trace data from input to output
- Verify type consistency
- Check transformation logic
- Validate error propagation

### Phase 4: Break Point Analysis

For each user flow, examine EVERY potential failure point:

**Data Validation Failures:**
- What if required fields are empty?
- What if data types are wrong?
- What if values are out of range?
- What if arrays are empty?
- What if objects have unexpected structure?

**Database Failures:**
- What if SQL query fails?
- What if transaction rollback is needed?
- What if foreign key constraint fails?
- What if unique constraint is violated?
- What if database connection is lost?

**API Failures:**
- What if API returns 400/500 error?
- What if API times out?
- What if response is malformed?
- What if authentication fails?
- What if rate limit is hit?

**Logic Failures:**
- What if conditional logic has edge case?
- What if loop terminates unexpectedly?
- What if recursive function hits base case wrong?
- What if async operation isn't awaited?
- What if promise rejection isn't caught?

**Integration Failures:**
- What if MCP server is unavailable?
- What if webhook delivery fails?
- What if external service is down?
- What if data format changes?
- What if version mismatch occurs?

### Phase 5: Edge Case Testing

**Empty States:**
- Empty arrays in SQL IN clauses
- Empty strings in required fields
- Null vs undefined handling
- Zero values in calculations
- Empty result sets from queries

**Boundary Cases:**
- Maximum string lengths
- Minimum/maximum numeric values
- Date ranges (past, future, timezone issues)
- Array length limits
- Nested object depth

**Concurrent Operations:**
- Multiple users editing same data
- Race conditions in async operations
- Database locking issues
- Cache invalidation problems

**Permission Scenarios:**
- Unauthorized access attempts
- Expired sessions
- Role-based access control
- Cross-tenant data access

### Phase 6: Feature-Specific Checks

**For Proposal Features:**
- Check MCP server proposal-quiz-tool endpoints
- Verify input/output data pipeline
- Test proposal creation, editing, deletion
- Verify permission checks for different roles
- Check quiz configuration loading
- Validate proposal state transitions

**For Survey/Feedback Features:**
- Check eligibility logic
- Verify submission flow
- Test coupon generation
- Validate webhook triggers
- Check duplicate submission prevention
- Verify data privacy compliance

**For Chat/Communication Features:**
- Check real-time message delivery
- Verify message persistence
- Test file upload/download
- Validate permission controls
- Check notification triggers

**For Admin Features:**
- Verify admin-only access controls
- Test bulk operations
- Check audit logging
- Validate data export functionality

**For Integration Features:**
- Test external API authentication
- Verify data synchronization
- Check error recovery mechanisms
- Validate retry logic

## Decision Framework

### GO Criteria (Feature is ready)
✅ All user flows work end-to-end
✅ Error handling covers all identified break points
✅ Edge cases are handled gracefully
✅ Data validation is comprehensive
✅ Authorization checks are in place
✅ Database queries are optimized and safe
✅ No SQL injection vulnerabilities
✅ Type safety is maintained throughout
✅ Logging and monitoring are adequate
✅ Documentation is complete
✅ Tests cover critical paths

### NO-GO Criteria (Feature has issues)
🚨 **CRITICAL Issues** (Must fix before deployment):
- SQL syntax errors
- Missing authorization checks
- SQL injection vulnerabilities
- Unhandled promise rejections
- Missing error handling in critical paths
- Data loss scenarios
- Security vulnerabilities

⚠️ **HIGH Priority Issues** (Should fix before deployment):
- Missing validation on user input
- Inadequate error messages
- Missing null/undefined checks
- Race conditions
- Performance bottlenecks
- Missing indexes on frequently queried columns

📋 **MEDIUM Priority Issues** (Fix in near term):
- Missing test coverage
- Incomplete logging
- Suboptimal code organization
- Missing documentation
- Code duplication

## Output Format

### Summary Section
```
FEATURE REVIEW: [Feature Name]
DATE: [Review Date]
REVIEWER: Claude Code
DECISION: ✅ GO | 🚨 NO-GO

OVERALL ASSESSMENT:
[2-3 sentence summary of feature quality]
```

### User Flows Section
```
USER FLOWS IDENTIFIED: [count]

Flow 1: [Flow Name]
User Path: [step-by-step description]
Touch Points:
  - Frontend: [components]
  - Backend: [routes/services]
  - Database: [tables/queries]
Break Points Analyzed: [count]
Status: ✅ Validated | ⚠️ Issues Found | 🚨 Critical Issues
```

### Issues Section
```
ISSUES FOUND: [total count]

🚨 CRITICAL Issues: [count]
  Issue #1: [Description]
    Location: [file:line]
    Impact: [What breaks]
    Fix: [Specific remedy]

⚠️ HIGH Priority Issues: [count]
  Issue #1: [Description]
    Location: [file:line]
    Impact: [What breaks]
    Fix: [Specific remedy]

📋 MEDIUM Priority Issues: [count]
  Issue #1: [Description]
    Location: [file:line]
    Impact: [What degrades]
    Fix: [Specific remedy]
```

### Code Quality Section
```
CODE QUALITY ASSESSMENT:

Error Handling: [score/10] - [comments]
Type Safety: [score/10] - [comments]
Security: [score/10] - [comments]
Performance: [score/10] - [comments]
Maintainability: [score/10] - [comments]
Test Coverage: [score/10] - [comments]
```

### Recommendations Section
```
RECOMMENDATIONS:

Immediate Actions (Required for GO):
1. [Action item]
2. [Action item]

Short-term Improvements:
1. [Action item]
2. [Action item]

Long-term Enhancements:
1. [Action item]
2. [Action item]
```

## Example Review Process

```
User requests: /project:feature-touchpoint-review csat-survey

Step 1: Map user flows
  - Proposal display (banner shows)
  - Eligibility check
  - Survey submission
  - Coupon display

Step 2: Identify touchpoints
  Frontend:
    - CSATSurveyBanner.tsx
    - SurveyModal.tsx
  Backend:
    - GET /proposal-op/:id
    - GET /proposals/:id
    - GET /api/csat/eligibility/:proposalId
    - POST /api/csat/response
    - GET /api/csat/coupon/:proposalId
  Database:
    - csat_responses table
    - system_settings table
  Services:
    - csat-survey-service.ts

Step 3: Systematic code review
  Read proposal-op-routes.ts
  Grep for "searchKeys" in server/routes
  Read csat-survey-service.ts
  Grep for SQL queries with IN clauses

Step 4: Break point analysis
  Discovered: Empty searchKeys array causes SQL error
  Location: proposal-op-routes.ts:634
  Impact: Proposal load fails when quiz keys are undefined
  Severity: CRITICAL

Step 5: Edge case testing
  Test: quizConfigKey = undefined, quizKey = undefined
  Result: searchKeys = []
  Query: setting_key IN ()
  Outcome: SQL syntax error

Step 6: Decision
  DECISION: 🚨 NO-GO
  Reason: Critical SQL error on empty searchKeys array
  Fix: Add guard: if (searchKeys.length > 0) before query
```

## Notes

- Be extremely thorough - check EVERY code path
- Don't assume anything works - verify with Grep and Read
- Consider malicious user input scenarios
- Think about concurrent users
- Review error messages for information leakage
- Check for proper cleanup in error cases
- Verify transaction rollback logic
- Consider backwards compatibility
- Check for breaking changes in APIs
- Validate database migration impact

## Completion Criteria

- All user flows documented and validated
- All touchpoints examined with Read/Grep
- All break points identified and tested
- All issues categorized by severity
- Final GO/NO-GO decision rendered with justification
- Specific remediation steps provided for all issues
- Review summary saved to `/docs/feature-reviews/[feature-name]-review-[date].md`