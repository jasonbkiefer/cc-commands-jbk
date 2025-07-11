Create a new slash command in the appropriate location.

Usage: /project:make-command <command-name> <scope>

Where:
- <command-name>: Name for your new command (e.g., "review", "test", "deploy")
- <scope>: Either "project" (for this project) or "user" (for all projects)

This will create a template command file that you can customize.

```bash
#!/bin/bash

# Parse arguments
COMMAND_NAME="$1"
SCOPE="${2:-project}"

if [ -z "$COMMAND_NAME" ]; then
    echo "Error: Command name is required"
    echo "Usage: /project:make-command <command-name> [project|user]"
    exit 1
fi

# Determine the target directory
if [ "$SCOPE" = "user" ]; then
    COMMAND_DIR="$HOME/.claude/commands"
else
    COMMAND_DIR=".claude/commands"
fi

# Create the directory if it doesn't exist
mkdir -p "$COMMAND_DIR"

# Create the command file
COMMAND_FILE="$COMMAND_DIR/$COMMAND_NAME.md"

# Check if command already exists
if [ -f "$COMMAND_FILE" ]; then
    echo "Error: Command '$COMMAND_NAME' already exists at $COMMAND_FILE"
    exit 1
fi

# Create the command template
cat > "$COMMAND_FILE" << 'EOF'
# Command: {COMMAND_NAME}

Describe what this command does here.

## Usage
/{SCOPE}:{COMMAND_NAME} [arguments]

## Example
/{SCOPE}:{COMMAND_NAME} example-arg

## Implementation

You can include:
- Instructions for Claude
- Bash commands to execute
- File references to include
- Extended thinking triggers

```bash
# Example bash command
echo "Running {COMMAND_NAME} command..."
```

## Files to Reference
- `path/to/relevant/file.py`
- `path/to/another/file.js`

## Notes
- Customize this template for your specific use case
- Remove sections you don't need
- Add any additional context or instructions
EOF

# Replace placeholders
sed -i "s/{COMMAND_NAME}/$COMMAND_NAME/g" "$COMMAND_FILE"
sed -i "s/{SCOPE}/$SCOPE/g" "$COMMAND_FILE"

echo "✅ Created new $SCOPE command: $COMMAND_NAME"
echo "📝 Edit the template at: $COMMAND_FILE"
echo ""
echo "To use your new command, type: /$SCOPE:$COMMAND_NAME"
```