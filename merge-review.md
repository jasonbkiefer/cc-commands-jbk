# Command: merge-review

follow these instructions. do not create a new command.

Review the current branch for conflicts with the development branch, resolve them if found, and create a pull request with detailed change summary.

## Usage
/merge-review

## Implementation

<extended-thinking>
1. If there are uncommited changes - commit them and push to origin
2. Review current branch for conflicts with development branch
3. If conflicts exist, research each one and suggest modifications
4. Once conflicts are resolved (or if none exist), create a pull request
5. Include detailed summary of all changes in PR
6. If the PR contians only admin/non-customer facing enhancements (and is not a bug ticket) then assign lable 'skip-qa' to the PR in Github

I need to:
- Fetch latest changes from remote
- Check for merge conflicts with development branch
- If conflicts exist, analyze them and provide resolution suggestions
- Create a comprehensive PR with all changes documented
</extended-thinking>

```bash
echo "🔍 Starting merge review for branch: $(git branch --show-current)"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

# Fetch latest changes
echo "📥 Fetching latest changes from remote..."
git fetch origin

# Get current branch name
CURRENT_BRANCH=$(git branch --show-current)
echo "📌 Current branch: $CURRENT_BRANCH"

# Check if we're on development branch
if [ "$CURRENT_BRANCH" = "development" ]; then
    echo "❌ Cannot merge development into itself!"
    exit 1
fi

# Test merge with development branch
echo -e "\n🔄 Testing merge with development branch..."
git merge-tree $(git merge-base HEAD origin/development) HEAD origin/development > /tmp/merge-test.txt 2>&1

# Check for conflicts
if grep -q "<<<<<<< " /tmp/merge-test.txt; then
    echo "⚠️  Merge conflicts detected!"
    echo -e "\n📋 Conflicted files:"
    git merge-tree $(git merge-base HEAD origin/development) HEAD origin/development | grep -E "^\+<<<<<<< " | sed 's/+<<<<<<< //' | sort | uniq
    
    echo -e "\n🔧 I'll analyze each conflict and suggest resolutions..."
else
    echo "✅ No merge conflicts detected!"
fi

# Get comprehensive change summary
echo -e "\n📊 Analyzing changes..."
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

# Get commit history
echo -e "\n📝 Commits in this branch:"
git log origin/development..HEAD --oneline --no-merges

# Get changed files
echo -e "\n📁 Files changed:"
git diff --name-status origin/development...HEAD

# Get statistics
echo -e "\n📈 Change statistics:"
git diff --stat origin/development...HEAD

echo -e "\n🤖 Preparing to create pull request..."
```

After analyzing conflicts (if any), I will:

1. **For each conflict**:
   - Read the conflicted file
   - Analyze both versions (current branch vs development)
   - Understand the context and purpose of each change
   - Suggest the best resolution based on:
     - Code functionality
     - Recent commit messages
     - File history
   - Apply the resolution

2. **Generate PR summary**:
   - review all /docs/change_log/pending files for descriptions of changed functionality
   - review all commits and see if other changes were made
   - Summarize the functionality changes that were introduced  
   - Include files changed with brief description of why
   - Testing recommendations
   - Any breaking changes or important notes

3. **Create the pull request** using:
   ```bash
   gh pr create --base development --title "[Branch Name] - Feature/Fix Description" --body "generated-summary"
   ```

4. Move all change_logs from the pending folder to the appropriate month in the completed folder

## Notes
- Will not push changes without user confirmation
- Provides detailed analysis before making any changes