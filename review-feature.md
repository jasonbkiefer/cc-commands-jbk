# Command: feature-review

ultrathink

Thoroughly research and analyze an issue or feature request, then propose a solution without implementing code.

## Usage
/feature-review <issue-description>

## Example
/feature-review The live chat system is disconnecting after 5 minutes of inactivity

## Implementation

When this command is invoked, you should:

1. **Research Phase**
   - Use all available search tools (Glob, Grep, Task) to understand the codebase
   - Read relevant files to understand current implementation
   - Look for related code patterns, configurations, and dependencies
   - Search for any existing error logs or related issues
   - use the mcp-db-tool to understand all data assoicated with the request

2. **Analysis Phase**
   - Ask clarifying questions if needed:
     - What specific behavior is expected vs actual?
     - Are there any error messages?
     - When did this issue start occurring?
     - What steps reproduce the issue?
   
3. **Investigation**
   - Trace through the code flow
   - Identify potential root causes
   - Consider edge cases and related systems
   - Check for recent changes that might have introduced the issue

4. **Solution Proposal**
   - In a new md file in /docs/to_do (include YYYY-MM-DD in file name)
   - Present a detailed solution plan
   - Explain the root cause
   - Outline implementation steps
   - Identify files that need modification
   - Suggest any testing approach
   - Note potential risks or considerations
   - Generally persue the simplest possible solution to minimize implementation risks


**IMPORTANT**: Do NOT write or modify any code. Only research, analyze, and propose solutions.

## Files to Reference
Based on the issue, dynamically determine which files to examine. Common starting points:
- `CLAUDE.md` - Project overview and architecture
- `server/` - Backend implementation
- `client/src/` - Frontend components
- `shared/` - Database schemas and types
- `package.json` - Dependencies and scripts
- `.env.example` - Configuration options

## Notes
- Be thorough in research before proposing solutions
- Ask questions when details are unclear
- Consider system-wide implications
- Focus on understanding before suggesting fixes
- Present findings in a clear, structured format
- do not suggest a timeline or state time to implement at all (ll coding will be done by and agent in minutes - not days or weeks)
- assume all coding will be done by an agent and prepare you responses with this in mind