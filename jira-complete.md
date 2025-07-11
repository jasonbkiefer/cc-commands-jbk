# Command: jira-complete

Pulls a JIRA ticket and completes the development work outlined in the ticket using best practices for this codebase. Runs comprehensive tests before presenting to the user for QA. If the ticket hasn't been updated with full documentation, also runs /jira-update on ticket before beginning coding. 

Always use the WishDesk project (WD)

## Usage
/jira-complete [ticket-number] | [search-query]

## Example
/jira-complete 71 (implies WD-71)
/jira-complete "implement user authentication"

## Implementation

### 1. Retrieve JIRA Ticket Information
- Use MCP Atlassian tools to fetch the ticket details
- Extract requirements, acceptance criteria, and technical details
- Identify the ticket number for reference

### 2. Analyze Requirements
- Parse the ticket description and comments
- Identify specific features to implement
- Determine testing requirements
- Check for any linked documentation or related tickets

### 3. Code Implementation
- Follow CLAUDE.md guidelines and project conventions
- Use existing patterns and components from the codebase
- Implement features according to the ticket requirements
- Add proper TypeScript types and interfaces
- Follow the project's file organization structure

### 4. Testing Strategy
- Run existing tests to ensure no regressions: `npm test:efficient`
- Write unit tests for new functionality
- Run type checking: `npm run check`
- Test the development server: `npm run dev`
- Verify all acceptance criteria are met

### 5. Documentation
- Update code comments where necessary
- Document any new APIs or components
- Update relevant documentation files if explicitly required

### 6. Update JIRA Ticket
- Add implementation notes to the ticket
- Update ticket status if applicable
- Run /jira-update if documentation is incomplete

## MCP Tools to Use
- `mcp__atlassian__searchJiraIssuesUsingJql` - Search for tickets
- `mcp__atlassian__getJiraIssue` - Get full ticket details
- `mcp__atlassian__addCommentToJiraIssue` - Add implementation notes
- `mcp__atlassian__transitionJiraIssue` - Update ticket status

## Project-Specific Considerations
- Database: Use appropriate credentials (WishDesk vs Sugarwish)
- Testing: Run `npm test:efficient` for faster feedback
- Code quality: Always run TypeScript checks
- Server changes: Remember to restart the dev server
- Git: Commit after feature completion if requested

## Workflow Steps
1. Fetch and analyze the JIRA ticket
2. Plan implementation using TodoWrite
3. Implement the solution following project conventions
4. Write and run tests
5. Verify all acceptance criteria
6. Update JIRA ticket with implementation details
7. Present completed work to user for QA

## Notes
- Always confirm database structure changes with user ("CONFIRMED" in all caps)
- Follow existing naming conventions (camelCase for TS, snake_case for DB)
- Use existing UI components from `client/src/components/ui/`
- Reference CLAUDE.md for project-specific guidelines
- Ensure MCP server is enabled for SWIM functionality