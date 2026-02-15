# Command: db-migration

Creates a new database migration file with corresponding schema and storage updates following the project's MySQL patterns.

## Usage
/project:db-migration <table_name> <field_definitions>

## Example
/project:db-migration users "email_verified BOOLEAN DEFAULT FALSE, last_login DATETIME"

## Implementation

Create a migration file to make database changes listed below using @initial_schema.sql as a template (review @migrations.md for context) and update @mysql-schema.ts and @database-storage.ts (remember that schema tables and fields are camelCase and database fields are snake_case).

**Database Details:**
- name: sugarwish_wishdesk
- env properties (DB_HOST_MANAGE,DB_DATABASE_MANAGE,DB_USERNAME_MANAGE,DB_PASSWORD_MANAGE)
- Database type: MySQL
- Follow existing MySQL migration patterns from the /migrations directory

**Migration Requirements:**
- Use MySQL-compatible syntax (not PostgreSQL)
- Include IF NOT EXISTS logic for idempotency
- Add appropriate indexes for searchable fields
- Follow naming convention: YYYY_MM_DD_HHMMSS_description.sql
- Use VARCHAR with appropriate length constraints (not TEXT)

**Schema Updates Needed:**
1. Add fields to the specified table definition in mysql-schema.ts
2. Update relevant insert/update schemas to include new fields
3. Update corresponding methods in database-storage.ts and storage.ts
4. Ensure proper camelCase to snake_case field mapping

**Reference existing patterns from:**
- @migrations/initial_schema.sql (for MySQL syntax patterns)
- @migrations/2025_06_16_add_performance_indexes.sql (for MySQL index syntax)
- @migrations/2025_06_10_113732_add_sw_id_to_users_table.sql (for field addition patterns)
- Existing table definitions for consistent field types and lengths

## Steps to Execute

1. First, get the current timestamp for the migration filename:
```bash
date "+%Y_%m_%d_%H%M%S"
```

2. Parse the arguments to extract:
   - Table name (first argument)
   - Field definitions (remaining arguments)

3. Create a new migration file in `/migrations/` with the pattern:
   - `YYYY_MM_DD_HHMMSS_add_fields_to_<table_name>_table.sql`

4. Generate the migration SQL:
   - Add ALTER TABLE statements with IF NOT EXISTS checks
   - Include appropriate indexes for searchable fields
   - Follow MySQL syntax patterns

5. Update `@shared/mysql-schema.ts`:
   - Add new fields to the table definition
   - Update insert/update schemas
   - Maintain camelCase naming

6. Update `@server/database-storage.ts`:
   - Add new fields to relevant methods
   - Ensure snake_case mapping in SQL queries
   - Update method signatures if needed

7. If applicable, update `@server/storage.ts`:
   - Mirror changes from database-storage.ts
   - Maintain interface consistency

## Migration Template

```sql
-- Migration: Add fields to {TABLE_NAME} table
-- Generated: {TIMESTAMP}

-- Add new columns
ALTER TABLE `{TABLE_NAME}`
{FIELD_DEFINITIONS};

-- Add indexes for searchable fields
{INDEX_DEFINITIONS}
```

## Files to Reference
- `migrations/initial_schema.sql`
- `migrations/2025_06_16_add_performance_indexes.sql`
- `migrations/2025_06_10_113732_add_sw_id_to_users_table.sql`
- `shared/mysql-schema.ts`
- `server/database-storage.ts`
- `server/storage.ts`

## Knowledge Registry Update

After creating the migration, update the org-knowledge registry with the schema change:

1. Call `mcp__swim-kb__upsert_org_knowledge` with:
   - title: "[table_name] table schema" (e.g., "users table schema")
   - summary: what changed and why
   - content: updated column list, new relationships, business meaning of new fields
   - source_type: "database-table"
   - source_repo: current repo name
   - source_file: migration file path
   - source_branch: current branch
   - category: "database-schema"
   - tags: ["database", "migration", table_name]
2. This keeps the knowledge registry in sync with schema changes

## Notes
- Always check existing table structure before adding fields
- Ensure migration is idempotent (can be run multiple times safely)
- Test migration on development database before committing
- Update any affected API endpoints or services