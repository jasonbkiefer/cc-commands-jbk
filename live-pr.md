Command: /cc-commands-jbk:live-pr

## Purpose
Create a NEW PR that merges development → live with a user-focused summary of all changes in this release.

## Implementation Steps

### 1. Generate the Diff
Run the following to see all changes between live and development:
```bash
git fetch origin
git diff origin/live...origin/development --stat
git diff origin/live...origin/development --name-only
```

### 2. Analyze Change Logs
Check for any change log entries that exist in development but not in live:
```bash
git diff origin/live...origin/development -- docs/change_log/
```

For each change log entry found, extract:
- **Title** - The feature name
- **Impact** - Who is affected
- **Overview** - The user-friendly description

### 3. Create User-Focused Release Summary
Compile the differences into a **user-friendly summary** (NOT developer-focused). Format for Slack:

```
🚀 *Release Summary*

*What's New:*
• [Feature/Fix title] - [Brief user impact explanation]
• [Feature/Fix title] - [Brief user impact explanation]

*Who's Affected:*
• [User group] - [What changes for them]

*Contributors:* @name, @name

*Jira Tickets:* TICKET-123, TICKET-456
```

### 4. Identify Contributors
```bash
git log origin/live...origin/development --format='%an' | sort | uniq
```

### 5. Find Jira Tickets
Search commit messages and change logs for Jira ticket references (patterns like ABC-123).

### 6. Create the PR
Create a PR from development → live with:
- Title: `Release: [Date] - [Brief summary]`
- Body: The user-focused Slack-ready summary from step 3

## Key Principles
- **Focus on USERS, not developers** - Describe what changes for the end user, not technical implementation details
- **Plain language** - Avoid jargon; write so any user can understand
- **Actionable** - Tell users what they can now do or what's fixed
- **Change logs are the source of truth** - Prioritize information from `/docs/change_log/` entries

## Output
Return the Slack-ready summary so it can be copy/pasted directly to notify users of the release.
