---
description: Re-authorize Google Drive MCP when it stops working
---

Try to refresh the Google Drive MCP token. If refresh fails, run full re-auth.

Steps:
1. First try the quick refresh: run `~/.config/mcp-gdrive/refresh-token.sh`
2. If that fails with "invalid_grant", run full re-auth: `~/.config/mcp-gdrive/reauth.sh`
3. Report success/failure to the user
4. Tell them: "Run /mcp → select gdrive → press 'd' to disable, then 'e' to enable"
