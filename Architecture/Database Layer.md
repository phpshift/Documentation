# Database Layer

PHPShift manages MySQL databases through two layers: a Python-side `DB` module for CLI operations (schema inspection, patch rollback, session management), and a PHP-side ORM provided by the `.system/` framework engine for runtime query execution.

## Python DB Module

The `DB` class (`modules/database.py`) is used by PHPShift's CLI during the development session - not by the running PHP project. Its responsibilities include:

- **Connection management** - Connects via SQLAlchemy + PyMySQL using `.env` credentials
- **Schema inspection** - Provides `DB.schema()` used by the `[[databaseSchema]]` collector, giving the AI an accurate picture of all existing tables
- **Patch rollback** - `DB.reserve()` snapshots pending changes; `DB.rollback()` reverts them if the developer rejects AI output
- **Database creation** - `DB.new(name)` runs `setup.sql` to initialize the database and create framework system tables
- **Direct operations** - `DB.insert()`, `DB.update()`, `DB.delete()`, `DB.query()` are available for CLI-side data operations

### DB Patch System

Every time a skill is about to generate code that may run SQL, PHPShift calls `DB.reserve()` to begin a tracked transaction. If the developer accepts the changes, `DB.clear()` finalizes them. If they reject, `DB.rollback()` reverts all database mutations made during that skill run.

## PHP Database ORM

At runtime, the PHP framework's `.system/` engine provides a database abstraction layer accessed through static methods. Generated page and API code uses these exclusively - never raw PDO or mysqli.

Common patterns in generated PHP:

```php
// Fetch records
$items = DB::select('SELECT * FROM tasks WHERE user = :uid', [':uid' => $userId]);

// Insert a record
$id = DB::insert('tasks', ['title' => $title, 'user' => $userId, 'created' => date('Y-m-d H:i:s')]);

// Update a record
DB::update('tasks', ['title' => $newTitle], 'id = :id', [':id' => $taskId]);

// Delete a record
DB::delete('tasks', 'id = :id', [':id' => $taskId]);
```

These methods are among the `[[phpMethods]]` collected by `Collectors.phpMethods()` and made available to the AI when generating code, ensuring correct usage every time.

## System Tables

When a new project database is initialized, PHPShift runs `setup.sql` which creates these framework-level tables:

| Table | Purpose |
|-------|---------|
| `user_files` | Tracks user-uploaded file references (path, mime, token) |
| `user_keys` | Stores named key-value pairs per user (sessions, tokens, settings) |
| `api` | Manages registered API access tokens with expiry |

These tables are used by the framework engine itself. Your application tables are created by the `db.sql` files generated alongside your pages.

## SQL Generation Rules

When PHPShift's AI generates SQL, it follows strict rules enforced in every prompt:

- No `CREATE DATABASE` statement (PHPShift handles database creation separately)
- Timestamps always use `DEFAULT NULL` or `DEFAULT CURRENT_TIMESTAMP` - never a fixed value
- Example records are included at the end of every schema file to make testing easy
- The MySQL version is passed to the AI via `[[databaseVersion]]` to prevent syntax incompatibilities

## Read Skills for Database

The `Read > Database` skill lets you query your database using natural language during a development session. You describe what data you want, and the AI generates a `statement.sql` and `params.json` which PHPShift then executes and displays inline. This is useful for debugging, inspecting data, or verifying that generated inserts worked correctly.
