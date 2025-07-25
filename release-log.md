# Command: release-log

Creates a release log that summarizes all changes from the change_log folder since the last release.

## Usage
/release-log

## Example
/release-log

## Implementation

For all change logs in the /docs/change_log/pending folder create a new markdown file in the /docs/release_log folder that summarizes all of the changes outlined in the /docs/change_log folder since the last release_log record. Include a summary at the top with the top 10 changes that impact users (no more than 10 words each)

Use this format (don't use markdown formatting):

```
RELEASE LOG [date/time]

[Title of Item 1 in all caps]
TYPE [Type of Item 1]
OVERVIEW:
[Overview of Item 1]

[Title of Item 2 in all caps]
TYPE [Type of Item 2]
OVERVIEW:
[Overview of Item 2]

... (continue for all items)
```

## Files to Reference
- `/docs/release_log/` - Directory where release logs are stored
- `/docs/change_log/pending/` - Directory containing individual change logs to summarize

## Notes
- Review every change log in the pending folder
- Summarize each change in the specified format
- Create a new file with naming pattern: YYYY-MM-DD_release.md
- when completed move all files from the `/docs/change_log/pending/` folder to the appropriate /change_log/YYYY-MM folder. 