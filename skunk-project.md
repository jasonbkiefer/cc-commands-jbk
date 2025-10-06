# Command: skunk-project

ultrathink

Fully research, document, plan, develop, and test a complete project idea autonomously without user interaction until completion.

## Usage
/skunk-project <project-description-and-requirements>

## Example
/skunk-project Create a ticket template system that allows users to save and reuse common ticket responses with variable placeholders

## Implementation

When this command is invoked, you should execute ALL phases autonomously:

### Phase 1: Branch Creation
- Extract a short project name from the description (2-3 words, kebab-case)
- Create a new git branch: `jason/skunk/[project-name]` from `development`
- Switch to the new branch

### Phase 2: Research Phase
- Use all available search tools (Glob, Grep, Task) to understand the codebase
- Read relevant files to understand current architecture and patterns
- Look for similar features or code patterns that can be referenced
- Use mcp-db-tool to understand database schema and related data
- Identify all integration points and dependencies
- Document key findings

### Phase 3: Documentation Phase
- Create comprehensive documentation in `/docs/to_do/YYYY-MM-DD-[project-name].md`
- Include:
  - **Project Overview**: Clear description of what will be built
  - **Requirements**: Detailed functional and technical requirements
  - **Architecture**: How it fits into existing system
  - **Database Schema**: Any new tables, columns, or relationships needed
  - **API Endpoints**: Backend routes and contracts
  - **UI Components**: Frontend components and user flows
  - **Dependencies**: External libraries or services needed
  - **Security Considerations**: Auth, permissions, data validation
  - **Testing Strategy**: How to validate the feature works

### Phase 4: Planning Phase
- Create detailed implementation plan with specific steps
- Identify all files to be created or modified
- Plan order of implementation (database → backend → frontend → tests)
- Note any potential risks or blockers
- Create a comprehensive todo list using TodoWrite tool

### Phase 5: Development Phase
Execute the plan autonomously:
- **Database Changes**:
  - Update wishdesk MySQL schema if needed 
  - Create migrations with /migration-create
- **Backend Implementation**:
  - Create/update API routes
  - Implement business logic
  - Add validation and error handling
- **Frontend Implementation**:
  - Create/update React components
  - Implement UI/UX
  - Add proper TypeScript types
- **Integration**:
  - Connect frontend to backend
  - Ensure all data flows correctly
- Mark todos as complete as you progress

### Phase 6: Testing Phase
- Start dev server if not running (`npm run dev`)
- Get base URL from environment variable WISHDESK_BASE_URL
- Use playwright-mcp to test all functionality at the WISHDESK_BASE_URL
- Login with username: jason, password: swdev123
- Test all user flows
- Verify UI renders correctly (take screenshots)
- Test error handling
- Validate data persistence using mcp-db-tool
- Fix any bugs discovered during testing

### Phase 7: Verification Phase
- Run `npm run check` to verify no TypeScript errors
- Run `npm test` to ensure no test failures
- Verify all todos are completed
- Create a summary of what was built

### Phase 8: Completion
- Commit all changes to git with descriptive commit message
- **ONLY THEN** stop and present to user:
  - Summary of what was built
  - Link to documentation
  - Screenshots of working feature
  - Next steps (PR creation, deployment notes)

## Critical Rules

1. **Autonomous Execution**: Do NOT stop for user input during phases 1-7
2. **Complete Implementation**: Feature must be fully functional before stopping
3. **Test Everything**: Use playwright to validate all functionality works
4. **Fix All Issues**: If bugs are found during testing, fix them immediately
5. **Use Existing Patterns**: Follow existing codebase patterns and conventions
6. **Database Safety**: when possible use existing db fields. user system_settings table as storage of properties instead of creating new tables or fields (use json)
7. **Track Progress**: Use TodoWrite to track all tasks
8. **Commit When Done**: Always commit working code to the branch

## Files to Reference
Based on the project, dynamically determine which files to examine. Common starting points:
- `CLAUDE.md` - Project overview and architecture
- `docs/claude/` - System architecture and guidelines
- `server/` - Backend implementation patterns
- `client/src/` - Frontend component patterns
- `shared/` - Database schemas and TypeScript types
- `package.json` - Available dependencies

## Notes
- This is a "skunk works" style autonomous development command
- The agent should work independently through all phases
- Only stop when feature is complete, tested, and working
- Present finished, production-ready code to the user
- If you encounter blockers, problem-solve autonomously or document why it can't proceed
- Assume all coding will be done by the agent in hours, not days
- Follow WishDesk coding standards and security guidelines
