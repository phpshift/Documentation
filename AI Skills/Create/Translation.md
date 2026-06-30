# Create > Translation

The `Create > Translation` skill adds full multi-language support to an existing page in one operation. It modifies HTML, CSS, JavaScript, and PHP files to integrate translation tags, then generates the translation JSON file for the requested language.

## What It Does

Given a page and a target language, the AI:

1. **Updates `page.html`** - Adds a `<translation languages="en,{lang}">` tag to the page (or appends the new language to an existing one). Adds `translate="{key}"` attributes to all hardcoded text elements.

2. **Updates `style.css`** - Adds `translation` and `translation img` CSS rules for the language switcher UI component.

3. **Updates `script.js`** - Wraps hardcoded strings in `App.lng(key, text)` calls.

4. **Updates `code.php`** - Wraps PHP hardcoded strings in `App::lng(key, text)` calls.

5. **Creates `Space/lng.{lang}.json`** - The translation file containing all key-value pairs for the requested language.

## Example Task

```
Add Georgian (ka) translation to the dashboard page
```

## Translation Key Format

Keys are unique, descriptive, and lowercase with underscores:

```html
<h1 translate="dashboard_title">My Dashboard</h1>
<button translate="add_task_btn">Add Task</button>
```

```json
// Space/lng.ka.json
{
    "dashboard_title": "ჩემი სამუშაო მაგიდა",
    "add_task_btn": "დავალების დამატება"
}
```

## Dynamic Translations

For strings with variables, the `{variable}` syntax is used:

```javascript
App.lng('welcome_msg', 'Hello, {name}!', {name: username});
```

```json
{ "welcome_msg": "გამარჯობა, {name}!" }
```

## Adding More Languages

Run `Create > Translation` again with a different language code. The skill detects the existing `<translation>` tag and appends the new language abbreviation to its `languages` attribute.

---

# Create > SEO

The `Create > SEO` skill generates `Space/seo.html` - the project-level SEO metadata file included in every page's `<head>`.

## What It Generates

A complete HTML snippet containing:

- Open Graph meta tags (`og:title`, `og:description`, `og:image`, `og:url`)
- Twitter Card tags
- Standard description and keyword meta tags
- Canonical URL
- Structured data (JSON-LD) where applicable

## Example Task

```
SEO for a recipe sharing platform called "RecipeHub" targeting home cooks, 
using the share.png image and focusing on keywords around easy recipes and cooking tips
```

## Note

The SEO file is shared across all pages. Page-specific SEO overrides should be set dynamically in individual `code.php` files using framework SEO helpers.

---

# Create > README

The `Create > README` skill generates or regenerates `README.md` at the project root. It reads your existing pages' `readme.md` files and the project `.md` briefing to produce a comprehensive, professional readme.

## What It Generates

- Project title and description
- Features list
- Tech stack
- Setup instructions
- Page/API overview
- Usage examples

## When to Use

- After completing an initial set of pages
- Before publishing to GitHub
- After major feature additions via `Edit > Project`

---

# Create > LICENSE

The `Create > LICENSE` skill installs a license file at the project root. PHPShift includes a library of common open source licenses and lets you select one interactively.

## Available Licenses

- MIT License
- Apache Software License 2.0
- GNU General Public License v3 (GPLv3)
- GNU Affero General Public License v3
- GNU Lesser General Public License v3
- Mozilla Public License 2.0
- BSD License
- ISC License

The selected license is populated with your project author name (from `PROJECT_AUTHOR` in `.env`) and the current year.

## Note

This skill is only shown in the menu if no `LICENSE` file exists yet. Once a license is installed, use `Edit > LICENSE` to change it or `Delete > LICENSE` to remove it.
