# Create > Page

The `Create > Page` skill generates a complete, working full-stack page: HTML structure, CSS styles, JavaScript interactions, PHP business logic, a database migration and documentation.

## Context the AI Receives

Before generating, PHPShift collects:

- Current database schema and MySQL version
- All existing page, API and cron file paths
- All available PHP methods (from `.system/` with `AI-USE` annotations)
- All available JavaScript `App` object methods
- CSS from an existing page (you choose which one as a styling reference)

This ensures new pages are consistent with your existing codebase.

## Generated Files

| File | Always? | Purpose |
|------|---------|---------|
| `readme.md` | Yes | Page goal, functionality and usage docs |
| `code.php` | Yes | PHP class with business logic (even if minimal) |
| `page.html` | If needed | HTML body structure |
| `style.css` | If needed | Page-specific responsive styles |
| `script.js` | If needed | jQuery/jQook front-end logic |
| `db.sql` | If needed | MySQL migration to apply immediately |
| `config.json` | Yes | Page metadata (name, group, composer deps) |

## Example Task

```
A private dashboard page showing the logged-in user's task count, 
recent activity feed and a quick-add task form
```

PHPShift sends the AI your database schema, existing PHP methods and a CSS example. The AI generates a complete dashboard page that uses your actual table names and PHP method signatures.

## Styling Reference

When creating a page, PHPShift asks "Select styling example" - choose an existing page whose CSS design you want the new page to match. If no pages exist yet, you can skip this.

## Access Group Auto-Generation

If your page uses a new access group (e.g., `premium`) that doesn't exist yet in `.access`, PHPShift automatically runs `Render > Group` to generate the access method.

## Database Migrations

If the AI generates a `db.sql` file, PHPShift opens it in VS Code and asks for confirmation before applying it to the database. If you decline the changes at the review step, the database changes are rolled back automatically.
