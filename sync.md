# Command: sync

Syncs the current branch with development by merging development into the current branch. Handles merge conflicts by prompting for resolution strategies and waits for user confirmation before proceeding.

## Usage
/project:sync

## Example
/project:sync

## Implementation

This command will:
1. Fetch the latest changes from the remote repository
2. Attempt to merge development into the current branch
3. If conflicts occur, analyze each conflict and recommend resolution strategies
4. Wait for user confirmation on each conflict resolution
5. Complete the merge after all conflicts are resolved

```bash
# Fetch latest changes
git fetch origin

# Get current branch name
CURRENT_BRANCH=$(git rev-parse --abbrev-ref HEAD)

echo "🔄 Syncing branch '$CURRENT_BRANCH' with development..."

# Check if we're already on development
if [ "$CURRENT_BRANCH" = "development" ]; then
    echo "❌ You're already on the development branch. Switch to your feature branch first."
    exit 1
fi

# Attempt to merge development
echo "📥 Merging development into $CURRENT_BRANCH..."
git merge origin/development
```

## Conflict Resolution Strategy

When conflicts are detected:
1. Identify all conflicted files
2. For each file, analyze the conflict and suggest one of:
   - **Keep current branch changes** - when your changes are more recent/important
   - **Accept development changes** - when development has critical updates
   - **Manual merge required** - when both changes are important and need combining
   - **Custom resolution** - provide specific merge strategy

## Files to Reference
- `.git/` - Git repository state
- Conflicted files as they appear

## Notes
- Always run this before creating a pull request to development
- Ensures your branch has the latest development changes
- Prevents merge conflicts in the pull request
- Creates a clean merge history