# Command: swim-tool

Creates a complete SWIM MCP tool with full implementation, including tool registry updates, types, and test interface.

## Usage
/project:swim-tool

## Example
/project:swim-tool

When you run this command, you'll be prompted to provide:
1. Tool name (e.g., "update-order-status")
2. Tool description
3. Input parameters and their types
4. Output structure
5. Implementation details

## Implementation

<thinking>
The user wants a comprehensive command that handles the entire process of creating a SWIM MCP tool. Based on my analysis, I need to:

1. Gather tool requirements from the user
2. Create the tool implementation file
3. Add types to the shared types file
4. Update the tool registry
5. Update the MCP server index
6. Update the test interface HTML
7. Run validation to ensure everything works

The command should handle all the boilerplate and structure, allowing the user to focus on the tool's business logic.
</thinking>

First, I'll gather detailed requirements for your new SWIM MCP tool:

```bash
echo "🚀 SWIM MCP Tool Generator"
echo "========================="
echo ""
```

### Step 1: Gather Tool Information

Please provide the following information for your new MCP tool:

1. **Tool Name** (kebab-case, e.g., "update-order-status"):
2. **Tool Description** (what does this tool do?):
3. **Input Parameters** (provide each parameter with name, type, required status, and description):
   - Example: `orderId: string (required) - The order ID to update`
   - Example: `status: string (required) - New status value`
   - Example: `notes?: string (optional) - Additional notes`
4. **Output Structure** (what does the tool return?):
   - Example: `{ success: boolean, orderId: string, newStatus: string, updatedAt: string }`
5. **Implementation Requirements**:
   - Which database tables/services will it access?
   - What validation is needed?
   - Any special error cases to handle?

### Step 2: Implementation Plan

Based on your requirements, I will:

1. **Create Tool Implementation** (`server/swim/tools/[tool-name].ts`)
   - Input validation
   - Business logic implementation
   - Error handling with ToolError
   - Logging with logToolCall
   - Return structured output

2. **Add Type Definitions** (`server/swim/shared/types.ts`)
   - Input interface
   - Output interface
   - Any additional types needed

3. **Update Tool Registry** (`server/swim/mcp-server/tool-registry.ts`)
   - Import new tool
   - Add to setupToolRegistry function
   - Register with MCP server
   - Add to getAvailableTools and getToolCapabilities

4. **Update MCP Server** (`server/swim/mcp-server/index.ts`)
   - Add case in callToolDirect method

5. **Update Test Interface** (`mcp-tool-test.html`)
   - Add tab button
   - Create form panel with inputs
   - Wire up test functionality

6. **Validate Implementation**
   - Check TypeScript compilation
   - Verify tool appears in registry
   - Test through mcp-tool-test.html interface

### Step 3: Execution

Once you provide the tool specifications, I will:

```bash
# Review existing SWIM implementation
echo "📋 Analyzing existing SWIM implementation..."

# Create all necessary files and updates
echo "🔨 Creating tool implementation..."
echo "📝 Adding type definitions..."
echo "📚 Updating tool registry..."
echo "🔧 Updating MCP server..."
echo "🧪 Updating test interface..."

# Run validation
echo "✅ Validating implementation..."
npm run check

echo "🎉 SWIM MCP tool created successfully!"
echo ""
echo "Next steps:"
echo "1. Review the generated files"
echo "2. Test the tool at http://localhost:3690/swim/mcp-tool-test.html"
echo "3. Integrate the tool into workflows as needed"
```

## Files to Reference
- `server/swim/tools/` - Existing tool implementations
- `server/swim/shared/types.ts` - Type definitions
- `server/swim/mcp-server/tool-registry.ts` - Tool registry
- `server/swim/mcp-server/index.ts` - MCP server
- `mcp-tool-test.html` - Test interface
- `server/swim/services/queue-monitor.ts` - Queue monitor integration

## Notes
- Ensure your tool follows the established patterns in existing tools
- Use ToolError class for consistent error handling
- Always include comprehensive logging
- Test thoroughly with edge cases
- Consider performance implications for database queries
- Document any special requirements or limitations

## Common Issues and Solutions

### Issue: 404 Error When Testing New Tool
**Symptom**: After creating a new tool, testing it returns a 404 error with "Network Error" and "Unexpected token '<'"

**Cause**: The server needs to be restarted to load new routes. Node.js/Express doesn't automatically reload route files when they're modified.

**Solution**:
1. **Always restart the development server** after adding new tools:
   ```bash
   # Stop the server with Ctrl+C
   # Restart with:
   npm run dev
   ```

2. **Add routes to BOTH test endpoints** to ensure tool is accessible:
   - MCP test route: `server/routes/admin-swim-mcp-test.ts`
   - Direct test route: `server/routes/admin-swim-test.js`

### Complete Checklist for Adding New Tools

To avoid the 404 error issue, follow this complete checklist:

1. **Create Tool Implementation** (`server/swim/tools/[tool-name].ts`)
   - ✅ Import from correct paths (no .js extensions in TypeScript)
   - ✅ Use `databaseConnector` from `../../mysql/database-connector`
   - ✅ Export tool with handler function

2. **Add Type Definitions** (`server/swim/shared/types.ts`)
   - ✅ Add Input interface
   - ✅ Add Output interface
   - ✅ Include optional error field in output

3. **Update Tool Registry** (`server/swim/mcp-server/tool-registry.ts`)
   - ✅ Import the tool
   - ✅ Add handler variable
   - ✅ Add tool loading in setupToolRegistry
   - ✅ Register tool with server.registerTool
   - ✅ Add to getToolCapabilities
   - ✅ Add to getAvailableTools array

4. **Update MCP Server** (`server/swim/mcp-server/index.ts`)
   - ✅ Add case in callToolDirect method

5. **Add Test Routes**
   - ✅ Add to `server/routes/admin-swim-mcp-test.ts` (MCP protocol)
   - ✅ Add to `server/routes/admin-swim-test.js` (direct testing)
   - ✅ Update availableTools array in health endpoint

6. **Update Test Interface** (`public/admin/swim/mcp-tool-test.html`)
   - ✅ Add tab button
   - ✅ Add tool panel with form

7. **Restart Server** ⚠️ **CRITICAL STEP**
   ```bash
   # This step is often forgotten but is required!
   # Stop server: Ctrl+C
   # Start server: npm run dev
   ```

### Debugging Tips

If you still get 404 errors after restarting:

1. **Check route registration**:
   ```bash
   # Verify the route was added
   grep -n "get-ecard-list" server/routes/admin-swim-mcp-test.ts
   ```

2. **Check server logs**:
   - Look for route loading messages
   - Check for any startup errors

3. **Test direct endpoint first**:
   - Try `/api/admin/swim/test/[tool-name]` before MCP endpoint
   - This helps isolate MCP-specific issues

4. **Verify authentication**:
   - Ensure you're logged in as admin
   - Check browser console for auth errors

### Best Practices to Avoid Issues

1. **Always add routes to both test files** simultaneously
2. **Run `npm run check` before testing** to catch TypeScript errors
3. **Document server restart requirement** in your PR/commit message
4. **Test both MCP and direct endpoints** to ensure full functionality
5. **Check browser Network tab** for actual error responses