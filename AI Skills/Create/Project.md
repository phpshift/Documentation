# Create > Project

The `Create > Project` skill generates a complete structural plan for your web project: a `README.md`, a database schema (`db.sql`), and individual markdown description files for every page, API, and cron job the AI determines is necessary.

## What It Generates

Given a plain-language project description, the AI produces:

1. **`README.md`** - Project overview: goal, pages, APIs, cron jobs, and usage summary
2. **`db.sql`** - Full MySQL schema with example records, ready to execute
3. **`page/{group}/{name}.md`** - Description file for each page
4. **`api/{version}.{name}.md`** - Description file for each API
5. **`cron/{name}.md`** - Description file for each cron job
6. **`config.json`** - Specifies the landing page

These markdown description files become blueprints - feed them one by one into `Create > Page`, `Create > API`, and `Create > CronJob` to generate actual code.

## When to Use It

Use `Create > Project` at the very start of a project to establish structure. It answers: "What pages, APIs, and cron jobs does this project need?"

For adding major new sections to an existing project, use `Edit > Project`.

## Example Task

```
A subscription recipe platform where users sign up, create and share recipes, 
comment on others' recipes, and access premium recipes with a paid plan. 
Include admin moderation tools.
```

The AI produces a complete plan covering auth pages, recipe CRUD, commenting, subscription handling, admin tools, and background cron jobs.

## What It Does Not Do

This skill generates **planning documents only** - no PHP, HTML, CSS, or JavaScript is written, and no database changes are applied. Actual code is generated per-unit via `Create > Page` etc.

## After Creation

Review the generated `README.md` and description files. Edit them to better reflect your vision before using them as code-generation input. The richer the descriptions, the higher quality the generated code will be.
