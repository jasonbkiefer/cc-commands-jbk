# Command: release-log

Creates a release log that summarizes all changes from the change_log folder since the last release.

## Usage
/release-log

## Example
/release-log

## Implementation

Check the date of the last /docs/release_log folder and create a new markdown file in the /docs/release_log folder that summarizes all of the changes outlined in the /docs/change_log folder since the last release_log record. 

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
- `/docs/change_log/` - Directory containing individual change logs to summarize

## Notes
- Find the date of the most recent release log
- Gather all change logs created after that date
- Summarize each change in the specified format
- Create a new file with naming pattern: YYYY-MM-DD_release.md