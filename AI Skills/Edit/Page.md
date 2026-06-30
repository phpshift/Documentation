# Edit > Page

The `Edit > Page` skill modifies an existing page. It receives the full current codebase of the target page and regenerates only the files that need to change.

## Context the AI Receives

- Full contents of the target page's existing files (`page.html`, `style.css`, `script.js`, `code.php`)
- Current database schema and MySQL version
- All available PHP methods
- All available JavaScript `App` methods
- CSS styling example

## How to Use

1. Select `Edit > Page` from the menu
2. Choose which page to edit from the list
3. Describe what needs to change

PHPShift sets the `VAR.target` variable to the selected page folder so the `[[codeBase]]` collector loads the right files.

## Example Task

```
Add a search and filter bar to the top of the page that filters the task list 
in real-time by title and status without page reload
```

The AI receives your existing page code and generates updated versions of only the files that need modification - leaving unchanged files intact.

## Iterative Refinement

The `Edit > Page` loop supports multi-turn refinement. If you select **Redo**, PHPShift asks you to adjust your description and retries. You can specify what was wrong:

```
The filter works but the results don't clear when I delete my search text. 
Also the status dropdown styling doesn't match the rest of the page.
```

## What Can Be Edited

- Any combination of HTML, CSS, JS, PHP on an existing page
- Database schema (via new `db.sql` if tables need changes)
- Access group logic (by adjusting `config.json` group settings)
- Composer dependencies (PHPShift installs new ones automatically)
