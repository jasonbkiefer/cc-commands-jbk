checkin all of my pending changes to my current branch in github
then push to origin

After pushing, check if any committed files have org-knowledge entries that should be refreshed:

1. Get the list of committed files from `git diff --name-only HEAD~1`
2. For each file, check `mcp__swim-kb__search_org_knowledge { query: "[filename]" }` for matching entries
3. If stale entries are found, mention: "Knowledge registry: [N] entries relate to your committed files and may need refreshing."