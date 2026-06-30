# Read > Database

The `Read > Database` skill lets you query your project's MySQL database using plain English. The AI translates your request into a SQL statement and parameters, executes them, and displays the results - no SQL knowledge required.

## How It Works

1. You describe the data you want
2. The AI receives your current database schema and generates `statement.sql` + `params.json`
3. PHPShift executes the query and displays the result inline in the terminal

## Example Tasks

```
Show me the 10 most recently created users with their email and created date
```

```
Count how many tasks are in each status category
```

```
Find all recipes that have more than 5 comments and were created this month
```

## Generated Files

The skill produces two temporary files before execution:

- **`statement.sql`** - The MySQL query (no markdown, no comments - pure SQL)
- **`params.json`** - Named parameters used in the query, or `{}` if none

## Safety

The AI is instructed to generate safe, parameterized queries only. No raw string interpolation, no destructive operations unless explicitly and unambiguously requested.

## Use Cases

- Debugging: verify that generated inserts or updates worked correctly
- Development: inspect data to understand what the AI-generated code is producing
- Quick lookups: check a user record, count rows, or inspect a join without leaving the terminal

## Note

This skill executes immediately - it does not go through the patch/rollback system since it is read-oriented. Destructive queries (DELETE, DROP, TRUNCATE) are possible if explicitly requested, so use care when describing your task.
