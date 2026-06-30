# Database Configuration

PHPShift uses MySQL (via XAMPP) for local development. All database connection details are stored in `.env` and managed through PHPShift's `DB` module.

## Connection Variables

```
DB_HOST="localhost"
DB_NAME="myapp_db"
DB_USER="root"
DB_PASS="your_password"
```

These are set during `phpshift new` and used by both the Python CLI layer (for schema inspection and patch management) and the PHP framework layer (for runtime query execution).

## Database Initialization

On first `phpshift start`, if the database doesn't exist, PHPShift creates it automatically by running `setup.sql`:

```sql
CREATE DATABASE IF NOT EXISTS `myapp_db` CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
USE `myapp_db`;
-- Creates system tables: user_files, user_keys, api
```

## Handling Existing Databases

If `DB_NAME` in `.env` is wrapped in brackets (e.g., `[myapp_db]`), PHPShift treats it as "not yet confirmed" and checks whether the database exists when starting. If it does, you're prompted:

- **Clear** - Drop and recreate the database (fresh start)
- **Keep** - Use the existing database as-is
- **Change** - Enter a different database name

This prevents accidental data loss when switching between project states.

## Database Schema Awareness

PHPShift continuously provides the AI with your current schema via `DB.schema()` → `[[databaseSchema]]`. This means:

- Generated SQL always uses your actual table and column names
- The AI knows what data types and constraints already exist
- Foreign key relationships are respected in generated queries

## Database Migrations

When a skill generates a `db.sql` file, PHPShift:

1. Writes the file to the page folder temporarily
2. Opens it in VS Code so you can review it
3. Asks "Confirm to update database" before running it
4. Applies it via `DB.submit()` if confirmed
5. Deletes the `.sql` file after successful application

If the skill run is rejected (Redo or No), the database changes are rolled back via `DB.rollback()`.

## Exporting the Schema

Use `More > ExportDatabaseSchema` to export your current database schema as a SQL file. Useful for:

- Sharing with collaborators
- Documenting the current state before a major migration
- Debugging schema issues by inspecting the full structure

## Production Database

For production, update `.env` on your server with the production database credentials. Set `PRODUCTION="1"` to enable production mode. PHPShift itself is not deployed to production - only the PHP framework and your project files are.

## Character Set

All PHPShift databases use `utf8mb4` with `utf8mb4_general_ci` collation by default, ensuring full Unicode support including emoji.
