Command: /cc-commands-jbk:staging

Deploy development to staging. This is ONE-WAY ONLY: development → staging.

**CRITICAL: NEVER merge staging into development. Never back-merge. Flow is always development → staging.**

Steps:
  - Check current branch
  - Make sure all changes in current branch are checked in. If not — stop and let user handle it
  - Create PR: development → staging (base: staging, head: development)
  - Description: Outline overall functionality changes between branches
  - Conflict check: Report conflicts and halt if found
  - Execute: Auto-merge immediately after PR creation
  - No confirmations needed
  - Return to branch user was on
