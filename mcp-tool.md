# Command: mcp-tool

Creates a new MCP (Model Context Protocol) tool configuration for Claude Code.

## Usage
/project:mcp-tool <tool-name> [tool-description]

## Example
/project:mcp-tool github-cli "Tool for interacting with GitHub repositories"
/project:mcp-tool web-scraper "Tool for scraping web content"

## Implementation

Parse the user input and create an MCP tool configuration:

```bash
#!/bin/bash

# Get the arguments from the command
FULL_ARGS="$@"

# Extract tool name (first word after command)
TOOL_NAME=$(echo "$FULL_ARGS" | awk '{print $1}')
# Extract description (everything after first word)
TOOL_DESC=$(echo "$FULL_ARGS" | cut -d' ' -f2-)

if [ -z "$TOOL_NAME" ]; then
    echo "Error: Tool name is required"
    echo "Usage: /project:mcp-tool <tool-name> [description]"
    exit 1
fi

# If no description provided, use a default
if [ "$TOOL_DESC" = "$TOOL_NAME" ]; then
    TOOL_DESC="MCP tool for $TOOL_NAME"
fi

# Convert tool name to lowercase and replace spaces/underscores with hyphens
TOOL_NAME_NORMALIZED=$(echo "$TOOL_NAME" | tr '[:upper:]' '[:lower:]' | tr '_' '-' | tr ' ' '-')

# Check if .mcp.json exists in the project root
PROJECT_ROOT="/home/jackk/SWAC"
MCP_CONFIG="$PROJECT_ROOT/.mcp.json"

if [ ! -f "$MCP_CONFIG" ]; then
    # Create new config file
    cat > "$MCP_CONFIG" << EOF
{
  "mcpServers": {
    "$TOOL_NAME_NORMALIZED": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-$TOOL_NAME_NORMALIZED"
      ]
    }
  }
}
EOF
    echo "✅ Created new .mcp.json with $TOOL_NAME_NORMALIZED tool"
else
    # Check if it's valid JSON
    if ! jq empty "$MCP_CONFIG" 2>/dev/null; then
        echo "❌ Error: .mcp.json is not valid JSON"
        echo "Please fix the JSON syntax before adding new tools"
        exit 1
    fi
    
    # Check if mcpServers exists
    if ! jq -e '.mcpServers' "$MCP_CONFIG" >/dev/null 2>&1; then
        # Add mcpServers object
        jq '. + {"mcpServers": {}}' "$MCP_CONFIG" > "$MCP_CONFIG.tmp" && mv "$MCP_CONFIG.tmp" "$MCP_CONFIG"
    fi
    
    # Check if tool already exists
    if jq -e ".mcpServers.\"$TOOL_NAME_NORMALIZED\"" "$MCP_CONFIG" >/dev/null 2>&1; then
        echo "⚠️  Tool '$TOOL_NAME_NORMALIZED' already exists in .mcp.json"
        echo "Do you want to overwrite it? (y/N)"
        read -r CONFIRM
        if [ "$CONFIRM" != "y" ] && [ "$CONFIRM" != "Y" ]; then
            echo "Cancelled."
            exit 0
        fi
    fi
    
    # Add the new tool to mcpServers
    jq ".mcpServers += {\"$TOOL_NAME_NORMALIZED\": {\"type\": \"stdio\", \"command\": \"npx\", \"args\": [\"-y\", \"@modelcontextprotocol/server-$TOOL_NAME_NORMALIZED\"]}}" "$MCP_CONFIG" > "$MCP_CONFIG.tmp" && mv "$MCP_CONFIG.tmp" "$MCP_CONFIG"
    
    echo "✅ Added $TOOL_NAME_NORMALIZED to .mcp.json"
fi

# Show the current configuration
echo ""
echo "📋 Current MCP tool configuration for $TOOL_NAME_NORMALIZED:"
jq ".mcpServers.\"$TOOL_NAME_NORMALIZED\"" "$MCP_CONFIG"

echo ""
echo "📝 Tool Details:"
echo "   Name: $TOOL_NAME_NORMALIZED"
echo "   Description: $TOOL_DESC"
echo "   NPM Package: @modelcontextprotocol/server-$TOOL_NAME_NORMALIZED"

echo ""
echo "🔧 Next steps:"
echo "1. If this tool needs environment variables, edit .mcp.json and add an 'env' section"
echo "2. If the NPM package name differs from the pattern above, edit the 'args' array"
echo "3. Restart Claude Code to load the new tool"
echo "4. Use /mcp to see all available MCP tools"

# Offer to show common patterns
echo ""
echo "💡 Common MCP tool patterns:"
echo "   - GitHub tool: Uses 'mcp-server-github' with GITHUB_TOKEN env var"
echo "   - Filesystem tool: Uses 'mcp-server-filesystem' with allowed paths"
echo "   - Web search: Uses various API keys for different search providers"
```

## Advanced Options

For tools that need environment variables or custom configurations, the command will prompt for additional setup:

```bash
# Example for adding environment variables
if [[ "$TOOL_NAME_NORMALIZED" == *"github"* ]] || [[ "$TOOL_NAME_NORMALIZED" == *"git"* ]]; then
    echo ""
    echo "🔑 This looks like a GitHub-related tool. Add a GITHUB_TOKEN? (y/N)"
    read -r ADD_TOKEN
    if [ "$ADD_TOKEN" = "y" ] || [ "$ADD_TOKEN" = "Y" ]; then
        echo "Enter your GitHub token (or press Enter to add placeholder):"
        read -r GITHUB_TOKEN
        if [ -z "$GITHUB_TOKEN" ]; then
            GITHUB_TOKEN="your-github-token-here"
        fi
        # Update the config with env vars
        jq ".mcpServers.\"$TOOL_NAME_NORMALIZED\".env = {\"GITHUB_TOKEN\": \"$GITHUB_TOKEN\"}" "$MCP_CONFIG" > "$MCP_CONFIG.tmp" && mv "$MCP_CONFIG.tmp" "$MCP_CONFIG"
        echo "✅ Added GITHUB_TOKEN to environment variables"
    fi
fi

# For filesystem tools
if [[ "$TOOL_NAME_NORMALIZED" == *"file"* ]] || [[ "$TOOL_NAME_NORMALIZED" == *"fs"* ]]; then
    echo ""
    echo "📁 This looks like a filesystem tool. Add allowed paths? (y/N)"
    read -r ADD_PATHS
    if [ "$ADD_PATHS" = "y" ] || [ "$ADD_PATHS" = "Y" ]; then
        echo "Enter allowed path (e.g., /home/user/projects):"
        read -r ALLOWED_PATH
        if [ -n "$ALLOWED_PATH" ]; then
            jq ".mcpServers.\"$TOOL_NAME_NORMALIZED\".args += [\"--allowed-path\", \"$ALLOWED_PATH\"]" "$MCP_CONFIG" > "$MCP_CONFIG.tmp" && mv "$MCP_CONFIG.tmp" "$MCP_CONFIG"
            echo "✅ Added allowed path: $ALLOWED_PATH"
        fi
    fi
fi
```

## Adding Custom MCP Tools to .mcp.json

For custom MCP tools (not from NPM), you can add them directly to the .mcp.json file:

```bash
# Using the mcp-tool command for custom local tools
/project:mcp-tool custom-tool-name "Description of your custom tool"

# Then manually edit .mcp.json to update the command path:
```

Example .mcp.json configurations for custom tools:

```json
{
  "mcpServers": {
    "test-runner": {
      "type": "stdio",
      "command": "node",
      "args": ["/path/to/your/mcp-tool/src/index.js"],
      "env": {}
    },
    "serp-database": {
      "type": "stdio",
      "command": "node",
      "args": ["/home/jackk/SERP/mcp-tools/serp-database/src/index.js"],
      "env": {}
    },
    "my-python-tool": {
      "type": "stdio",
      "command": "python",
      "args": ["/path/to/your/mcp-tool/main.py"],
      "env": {
        "API_KEY": "abc123",
        "DEBUG": "true"
      }
    }
  }
}
```

After adding the tool to .mcp.json:
1. Restart Claude Code to load the new tool
2. Use `/mcp` in Claude Code to see and interact with your tools

## Updating CLAUDE.md

After adding a new MCP tool, update your project's CLAUDE.md file to document it:

```markdown
## MCP Tools Available

### test-runner
**Purpose**: Run tests in the SERP project with optional filters
**Commands**:
- `run_tests`: Run tests with type filter (all/backend/frontend)
- `run_specific_test_file`: Run a specific test file
- `check_test_coverage`: Check test coverage

**Usage Example**:
```
Use the test-runner MCP tool to run backend unit tests
```
```

This helps Claude understand what tools are available and how to use them effectively.

## Critical MCP Tool Implementation Requirements

**🚨 MANDATORY PATTERNS FOR WORKING MCP TOOLS:**

### 1. Correct Imports
```javascript
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';
import {
  CallToolRequestSchema,
  ListToolsRequestSchema,
} from '@modelcontextprotocol/sdk/types.js';
// DO NOT import zod from MCP SDK - it doesn't exist there!
```

### 2. Server Initialization Pattern
```javascript
const server = new Server(
  {
    name: 'your-tool-name',
    version: '1.0.0',
  },
  {
    capabilities: {
      tools: {},
      resources: {},  // REQUIRED
      prompts: {},    // REQUIRED
    },
  }
);
```

### 3. Handler Setup Pattern
```javascript
// List tools handler
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      // tool definitions here
    ]
  };
});

// Tool call handler  
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { params } = request;  // CRITICAL: use params!
  const { name, arguments: args } = params;
  // implementation here
});
```

### 4. Connection Pattern
```javascript
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
}

main().catch(console.error);
```

**⚠️ COMMON MISTAKES THAT BREAK MCP TOOLS:**
- Using `'tools'` instead of `ListToolsRequestSchema` 
- Using `'tools/call'` instead of `CallToolRequestSchema`
- Accessing `request.name` instead of `request.params.name`
- Missing `resources` and `prompts` in capabilities
- Importing zod from MCP SDK (use direct dependency)

## Notes
- This command modifies /home/jackk/SWAC/.mcp.json directly for all MCP tools
- For NPM-based tools, it automatically sets up the npx command
- For custom tools, manually edit .mcp.json after initial creation
- It checks for existing tools and prompts before overwriting
- Automatically suggests common patterns for known tool types
- Uses jq for safe JSON manipulation
- Always backs up the config before making changes
- Remember to document new tools in CLAUDE.md for better context
- **ALWAYS follow the exact patterns above or your MCP tool will fail to load**