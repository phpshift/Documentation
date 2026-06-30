# Edit > Project

The `Edit > Project` skill updates an existing project plan when you need to add, change, or remove features across multiple pages. It works like `Create > Project` but with full awareness of what already exists.

## Context the AI Receives

- Descriptions of all existing pages (from their `readme.md` files)
- Current database schema and MySQL version

## What It Generates

1. **`strategy.md`** - A strategic overview of what needs to change and why
2. **`db.sql`** (if schema changes are needed) - Ready-to-execute migration SQL
3. **`{group}.{page}.md`** description files for pages that need updates

## Example Task

```
Add a social features layer: users should be able to follow each other, 
like recipes and see a social feed of activity from people they follow
```

The AI analyzes your existing pages and generates targeted update descriptions only for pages that need to change, plus the new pages/APIs required.

## After Generation

Use the generated `.md` description files as input for `Edit > Page` on each affected page to generate the actual code changes.
