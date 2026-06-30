# Create > README

The `Create > README` skill generates a professional `README.md` at the project root. It reads your project's existing page documentation files and the `.md` briefing to produce a comprehensive developer-facing readme.

## What It Generates

- Project title, description, and tagline
- Feature highlights
- Technology stack overview (PHP, MySQL, jQuery, jQook)
- Local setup instructions
- Pages and API route overview
- Usage examples

## When to Use

- After completing your initial set of pages and APIs
- Before publishing the repository to GitHub or GitLab
- After adding major new functionality via `Edit > Project`

## Context the AI Uses

The skill reads:
- Your `.md` project briefing file
- `readme.md` files from all existing pages
- Project metadata from `.env` (name, author, production domain)

## Note

This skill is only shown if no `README.md` exists yet. Once created, use `Edit > README` to update it or `Delete > README` to remove it.
