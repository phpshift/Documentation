# Patch System

The patch system is what makes PHPShift's AI generation safe to experiment with. Every skill run is wrapped in a snapshot-and-rollback mechanism covering both filesystem and database changes, so a bad AI response never leaves your project in a broken state.

## Why It Exists

AI-generated code isn't always perfect on the first try. Without a safety net, a flawed generation could overwrite working files or insert bad data into your database with no easy way back. The patch system makes every skill run fully reversible.

## How File Patching Works

Before a skill writes any files:

1. PHPShift identifies which files/folders the skill is about to touch
2. Current contents (or absence) of those paths are recorded in memory
3. The skill executes and writes new files
4. If you choose **Redo** or **No**, the original state is restored exactly:
   - Modified files are reverted to their prior content
   - New files that didn't exist before are deleted
   - Deleted files (in Delete skills) are restored

## How Database Patching Works

The Python `DB` module provides three key methods:

- **`DB.reserve()`** - Called before a skill begins; starts tracking subsequent database operations
- **`DB.clear()`** - Called when you confirm (Yes); finalizes all staged changes permanently
- **`DB.rollback()`** - Called when you reject (Redo/No); reverts all database mutations made since `reserve()`

This means a `db.sql` migration that creates a table, inserts data, or alters a schema can be completely undone if you're not happy with the accompanying code.

## What Gets Patch-Tracked

| Category | Tracked |
|----------|---------|
| Page files (html/css/js/php/readme) | Yes |
| API files | Yes |
| Cron files | Yes |
| Database schema changes | Yes |
| `.access` file changes | Yes |
| `.env` new variable additions | Yes |
| Composer package installations | Yes (uninstall on rollback) |
| Git commits | No - commits happen only after final confirmation |

## What Is Not Rollback-Safe

- **`Read > Database`** queries execute immediately and are not part of the patch system, since they're read operations by default (though a destructive query you explicitly request will run for real)
- **`Read > Git`** commands execute immediately for the same reason
- **Manual file edits** you make outside of PHPShift skills are not tracked

## Practical Implications

Because of the patch system, you can be liberal with **Redo**. If a generated page is close but not quite right, don't manually fix it - describe the correction and let the AI regenerate. The rollback ensures no half-finished, inconsistent state accumulates from failed attempts.

## Manual Recovery

If you ever need to manually verify rollback was clean, check your Git status - since uncommitted changes from a rejected skill run should leave your working directory exactly as it was before the skill started (assuming you commit after every accepted change, which PHPShift encourages).
