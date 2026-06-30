# Render > Page

The `Render > Page` skill re-renders an existing page from scratch - regenerating all its files while preserving the page's identity (name, group) and staying consistent with your current database schema and PHP method surface.

## Difference from Edit > Page

| | Edit > Page | Render > Page |
|--|------------|--------------|
| **Approach** | Modifies specific parts of existing code | Regenerates all files from your description |
| **Starting point** | Existing code is context | Existing readme.md is context |
| **Use when** | Making targeted changes | Page code is outdated or needs a full rewrite |
| **Preserves** | Most of the existing structure | Page name and group only |

Use **Edit > Page** for incremental improvements. Use **Render > Page** when a page's code has drifted too far from what you need and a clean regeneration makes more sense than patching.

## How It Works

1. You select the page to re-render from the list
2. PHPShift loads the page's `readme.md` as the base description
3. You can provide additional instructions to adjust the render
4. The AI regenerates all page files fresh, using current DB schema, PHP methods, and a styling reference

## Example Task

```
Re-render the dashboard page with an updated layout - 
move the activity feed to the right sidebar and make the stats cards larger
```

## Context the AI Receives

- The page's existing `readme.md` (goal, functionality, usage)
- Current database schema and MySQL version
- All available PHP methods and JS App methods
- CSS from a selected styling reference page
- List of all project files

## After Rendering

Review the regenerated files and confirm or roll back as usual. If you confirm, the old files are replaced by the fresh render.
