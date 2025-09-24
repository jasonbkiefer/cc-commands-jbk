Command: /cc-commands-jbk:staging
  - check current branch
  - make sure all changes in current branch are check it. if they are not - stop and mention allow user to do that
  - Create PR: development → staging
  - Description: Outline overall functionality changes between branches
  - Conflict check: Report conflicts and halt if found
  - Execute: Auto-merge immediately after PR creation
  - No confirmations needed
  - return to branch user was on
