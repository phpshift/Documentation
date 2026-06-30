# Delete > Page

Removes a page and all its associated files from the project.

## What Gets Removed

The entire `Pages/{group}.{name}/` folder including `page.html`, `style.css`, `script.js`, `code.php` and `readme.md`.

## Process

1. PHPShift lists all existing pages
2. You select the page to delete
3. Confirm the action
4. The folder and all its contents are removed
5. A Git commit opportunity is offered

## Rollback

File deletions are tracked by the patch system. If you decide to undo before ending the session, roll back via the `Redo` option.

## Database Tables

Deleting a page does **not** drop associated database tables. If you want to clean up tables, use `Read > Database` to run the appropriate DROP queries.

## Menu Visibility

Only shown when custom pages exist (the default `public.phpshift` page is excluded).
