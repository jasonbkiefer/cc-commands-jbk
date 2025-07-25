# Command: jira-ticket

Creates a new Jira ticket in the WD project using the specified format with component, difficulty, and impact fields.
DO NOT CODE. This is ticket research, writing and posting only. No fixing. 

## Usage
/project:jira-ticket [ticket description and requirements]

## Example
/project:jira-ticket Fix the live chat notification system to show proper sender names instead of email addresses

## Implementation

This command will create a Jira ticket with:
- Component assignment using customfield_10327 (array of strings with hyphens instead of spaces)
- Difficulty rating (1-10 scale) where 10 is a major, multiweek project and 1 is a text change
- Impact assessment (1-10 scale) where 1 is almost no impact and 10 is a feature potentially adding millions of dollars in revenue
- Comprehensive PRD-style description with technical requirements

### Component Options (use hyphens instead of spaces):
- CRM
- Design-Suite
- SWIM-MCP
- SWIM-Workflows
- Customer-Support
- Knowledgebase
- Authentication
- Proposals
- User-Admin

### Required Fields:
```json
{
  "customfield_10327": ["Component-Name"],  // Component field (use hyphens)
  "customfield_10324": 6,                   // Difficulty: 1-10 scale
  "customfield_10323": 7                    // Impact: 1-10 scale
}
```

I will:

1. **Research Phase**: Thoroughly analyze the codebase to understand the feature/system in question
   - Search for existing implementations and related code
   - Identify current architecture and patterns
   - Understand dependencies and integration points
   - Map out the data flow and component relationships
2. **Impact Analysis**: Based on codebase research, identify:
   - **Files that will need to be edited** (with specific file paths)
   - **Pages/components that will be changed** (frontend impact)
   - **API endpoints that need modification** (backend impact)
   - **Database schema changes** (if applicable)
   - **Third-party integrations affected** (if any)
3. **Component Classification**: Determine the most appropriate component based on research findings
4. **Difficulty Assessment**: Rate difficulty (1-10) based on:
   - Number of files requiring changes
   - Complexity of existing architecture
   - Integration touchpoints identified
   - Testing requirements
5. **Impact Evaluation**: Rate impact (1-10) considering:
   - User-facing changes identified
   - Business process improvements
   - Technical debt reduction
   - Performance implications
6. **PRD Creation**: Generate comprehensive description including:
   - Technical requirements based on codebase analysis
   - Specific file modification list
   - UI/UX changes required
   - Testing strategy
7. **Clarification**: Ask targeted questions if research reveals ambiguities
8. **Ticket Creation**: Use Atlassian MCP tools to create the ticket with all findings

## Files to Reference
- `.cursor/rules/jadd.mdc` - Jira ticket format specification

## Notes
- Every ticket should be assigned one of the listed components using **customfield_10327** (use hyphens instead of spaces)
- Component field uses customfield_10327 as an array of strings
- Difficulty field uses customfield_10324 (1-10 scale)
- Impact field uses customfield_10323 (1-10 scale)
- Always ask clarifying questions if the component, difficulty, or impact are unclear
- Create PRD for ticket including technical requirements in the description