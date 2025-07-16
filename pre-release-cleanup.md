# Command: pre-release-cleanup

Performs a comprehensive pre-release cleanup of the codebase, removing test files, migration scripts, and fixing hardcoded credentials.

## Usage
/project:pre-release-cleanup

## Example
/project:pre-release-cleanup

## Implementation

This command will:
1. Identify and remove standalone test .js files (excluding those referenced by HTML test pages)
2. Remove .js files used for database migrations and table modifications
3. Search for and fix hardcoded database credentials that should use environment variables
4. Provide a comprehensive report of all changes made

## Protected Files (DO NOT REMOVE)
The following files are explicitly protected and should not be removed:
- `server/mysql/user-list-route.js` - MySQL integration route
- `server/mysql/mysql-integration-v2.js` - Core MySQL integration
- `server/mysql/database-connector.js` - Core infrastructure, heavily referenced
- `shared/db-mysql.js` - Drizzle adapter, still in use
- Any .js test files that are referenced by HTML pages

## Cleanup Steps

1. **Find Test Files**: Search for .js test files that are NOT referenced by HTML pages
   - Look for files with patterns like `*.test.js`, `*.spec.js`, `test-*.js`
   - Check if they're imported or referenced in HTML files before removal
   - Ask for confirmation before removing any test files

2. **Find Migration Scripts**: Identify .js files used for database operations
   - Look for files containing migration-related keywords
   - Check for files with database schema modifications
   - Verify they're not part of the active codebase before removal
   - Leave all files in the /migrations folder

3. **Fix Hardcoded Credentials**: Search for database credentials in code
   - Look for patterns like passwords, connection strings, API keys
   - Replace with appropriate environment variable references
   - Update the code to use `process.env.VARIABLE_NAME`
   - SW_* and DB_* are the primary concerns

4. **Generate Report**: After all changes, provide a summary including:
   - List of files removed with reasons
   - List of credential fixes applied
   - Any files that were suspicious but kept (with reasons)
   - Recommendations for further cleanup if needed

## Search Patterns
- Test files: `**/*.test.js`, `**/*.spec.js`, `**/test-*.js`, `**/tests/**/*.js`
- Migration files: Files containing keywords like "migration", "migrate", "alter table", "create table", "drop table"
- Hardcoded credentials: Patterns like `password:`, `PASSWORD=`, connection strings with credentials, API keys

## Notes
- Always ask for confirmation before removing files that might be important
- Check for file references in both JavaScript/TypeScript and HTML files
- Preserve any files that are part of the build process or runtime
- Be extra cautious with files in `server/mysql/` directory as many are core infrastructure