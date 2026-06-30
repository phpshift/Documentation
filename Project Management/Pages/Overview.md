# Pages Overview

Pages are the primary building block of a PHPShift project. Each page is a self-contained, full-stack unit: a folder containing HTML structure, CSS styles, JavaScript interactions, PHP business logic and a documentation file.

## What a Page Is

A page in PHPShift corresponds to a single URL route in your application. When a visitor navigates to `/dashboard`, the framework routes the request to the `Pages/private.dashboard/` folder and serves its HTML through `code.php`.

Pages are not templates or partials - each one is a complete, independently functional unit. However, they share:

- Global assets from `.assets/` (base CSS, global JS, jQuery, jQook)
- PHP methods from `.system/` (App helper, DB ORM, access control)
- SEO metadata from `Space/seo.html`
- Translation keys from `Space/lng.*.json`

## Page Lifecycle

1. **AI generates the page** via `Create > Page` or `Render > Page`
2. **Files are written** to `Pages/{group}.{name}/`
3. **DB migration** (`db.sql`) is optionally applied
4. **Access group** method is auto-generated in `.access` if the group is new
5. **Developer confirms** or rolls back
6. **Page is live** at `/{name}` (not `/{group}/{name}`)

Note: Pages are accessed by their **name only** in URLs - the group is invisible to visitors. `/edititem` not `/public/edititem`.

## Page Files

| File | Required | Purpose |
|------|----------|---------|
| `readme.md` | Yes | AI-maintained documentation of page goal, functionality, usage |
| `code.php` | Yes | PHP class with business logic methods (even if just an empty class) |
| `page.html` | If needed | HTML structure (no `<html>`, `<head>`, or asset links) |
| `style.css` | If needed | Scoped responsive styles for this page |
| `script.js` | If needed | jQuery/jQook interaction logic |
| `db.sql` | Temporary | MySQL migration - deleted after being applied |
| `config.json` | Temp | Metadata used by PHPShift during generation - not stored permanently |

## Access Groups

The `{group}` prefix in the folder name determines which users can access the page. Groups are defined as static methods in the `.access` PHP class:

```php
class Access {
    public static function public() { return true; }
    public static function private() { return App::isLoggedIn(); }
    public static function admin() { return App::isAdmin(); }
}
```

When you create a page with a new group name, PHPShift detects that the group method doesn't exist and automatically runs the `Render > Group` skill to generate it.

## The readme.md File

Every page's `readme.md` is AI-generated and AI-readable. It serves as:

- **Developer documentation** you can read to understand the page
- **Context source** - when editing or rendering a page, the `[[pages]]` collector reads all `readme.md` files to give the AI project-wide page awareness
- **Project overview source** - if no `README.md` exists, the `[[aboutProject]]` collector assembles content from page readme files

Keep `readme.md` accurate - it's the AI's memory of what each page does.
