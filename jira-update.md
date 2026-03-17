# Command: jira-update

Updates an existing Jira ticket by evaluating and enhancing it with comprehensive technical analysis based on codebase research. Always search the WD (wishdesk) tickets using atlassian mcp. 

## Usage
/jira-update [ticket-id]

## Example
/jira-update 18

## Implementation

This command will:

1. **Retrieve Existing Ticket**: Fetch the current Jira ticket details using the provided ticket ID
1b. **Search Org-Knowledge**: Before deep codebase research, search the knowledge registry for existing context:
   - Call `mcp__swim-kb__search_org_knowledge { query: "[feature/system from ticket]" }` to find existing architectural knowledge
   - Use findings to accelerate research and avoid re-discovering known patterns
2. **Research Phase**: Thoroughly analyze the codebase to understand the feature/system mentioned in the ticket
   - Search for existing implementations and related code
   - Identify current architecture and patterns
   - Understand dependencies and integration points
   - Map out the data flow and component relationships
3. **Impact Analysis**: Based on codebase research, identify:
   - **Files that will need to be edited** (with specific file paths)
   - **Pages/components that will be changed** (frontend impact)
   - **API endpoints that need modification** (backend impact)
   - **Database schema changes** (if applicable)
   - **Third-party integrations affected** (if any)
4. **Component Validation**: Verify the assigned component is appropriate based on research findings
5. **Difficulty Re-assessment**: Re-evaluate difficulty (1-10) based on:
   - Number of files requiring changes
   - Complexity of existing architecture
   - Integration touchpoints identified
   - Testing requirements
6. **Impact Re-evaluation**: Re-assess impact (1-10) considering:
   - User-facing changes identified
   - Business process improvements
   - Technical debt reduction
   - Performance implications
7. **Enhanced Description**: Update ticket description with:
   - Technical requirements based on codebase analysis
   - Specific file modification list
   - UI/UX changes required
   - Testing strategy
   - Implementation approach
8. **Update Ticket**: Use Atlassian MCP tools to update the ticket with enhanced information

9. **Save to Knowledge Registry**: If research revealed important architectural insights:
   - Save key findings as org-knowledge entries via `mcp__swim-kb__upsert_org_knowledge`
   - Focus on reusable knowledge (architecture, component relationships) not ticket-specific details

### Component Options (for validation):
- CRM
- Design-Suite
- SWIM-MCP
- SWIM-Workflows
- Customer-Support
- Knowledgebase
- Authentication
- Proposals
- User-Admin

### Updated Fields:
```json
{
  "customfield_10327": ["Component-Name"],  // Updated component if needed
  "customfield_10324": 6,                   // Re-assessed difficulty: 1-10 scale
  "customfield_10323": 7                    // Re-assessed impact: 1-10 scale
}
```

## Files to Reference
- `.claude/commands/jira-ticket.md` - Base analysis methodology
- `.cursor/rules/jadd.mdc` - Jira ticket format specification

## Notes
- Will preserve existing ticket information while enhancing with technical details
- Re-evaluates component, difficulty, and impact based on actual codebase analysis
- Adds comprehensive implementation roadmap to ticket description
- Uses the same rigorous analysis process as jira-ticket command
- Requires valid Jira ticket ID that exists in the WD project