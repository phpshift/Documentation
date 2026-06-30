# Create > SEO

The `Create > SEO` skill generates `Space/seo.html` - the project-level SEO metadata file included in every page's `<head>`.

## What It Generates

A complete HTML snippet containing:

- Open Graph meta tags (`og:title`, `og:description`, `og:image`, `og:url`)
- Twitter Card tags
- Standard `<meta name="description">` and keyword tags
- Canonical URL tag
- JSON-LD structured data where relevant

## Example Task

```
SEO for a recipe platform called "RecipeHub" targeting home cooks. 
Keywords: easy recipes, cooking tips, meal planning. 
Use share.png as the social image.
```

## File Location

`Space/seo.html` is automatically included in every page's `<head>` by the framework. There is no need to reference it manually.

## Project vs. Page SEO

This file sets global defaults for all pages. For page-specific SEO overrides (different title, different description per page), use PHP `App` helper methods inside individual `code.php` files to dynamically override the meta tags for that route.

## After Creation

Preview the SEO output by inspecting `<head>` in your browser on any page. The `Space/seo.html` file is editable manually or via the `Edit > SEO` skill.

## Sitemap

The framework auto-generates `Space/sitemap.xml` with the current date when pages are added. The `.htaccess` serves it at `/sitemap.xml` automatically.
