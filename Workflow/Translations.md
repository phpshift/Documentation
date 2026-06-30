# Translations

PHPShift's translation system makes internationalizing a PHP project a one-step AI operation instead of a manual, error-prone process of touching every layer of the stack by hand.

## The Translation Workflow

```
1. Build your page in your primary language (usually English)
2. Run Create > Translation, specify the target language
3. AI updates HTML, CSS, JS, and PHP to support the new language
4. AI generates the Space/lng.{lang}.json file
5. Repeat for additional languages as needed
```

## What Gets Touched

| Layer | Change |
|-------|--------|
| `page.html` | `<translation languages="en,ka">` tag added; `translate="key"` attributes on text elements |
| `style.css` | Language-switcher UI styles added |
| `script.js` | Hardcoded strings wrapped in `App.lng(key, defaultText, vars)` |
| `code.php` | Hardcoded strings wrapped in `App::lng(key, defaultText, vars)` |
| `Space/lng.{lang}.json` | New file with all translated key-value pairs |

## The Translation Tag

The `<translation>` tag tells the framework which languages a page supports and renders a language switcher UI:

```html
<translation languages="en,ka,de"></translation>
```

The framework reads the visitor's preferred language (from a cookie, URL parameter, or browser header) and serves the matching `Space/lng.{lang}.json` values wherever `translate` attributes or `lng()` calls appear.

## Static vs. Dynamic Translations

**Static (HTML):**
```html
<h1 translate="page_title">Dashboard</h1>
```

**Dynamic (JavaScript):**
```javascript
const msg = App.lng('items_count', '{count} items found', {count: total});
```

**Dynamic (PHP):**
```php
$subject = App::lng('email_subject', 'Welcome, {name}!', ['name' => $username]);
```

The `{variable}` placeholder syntax works consistently across both JS and PHP `lng()` calls.

## Translation File Format

```json
{
    "page_title": "მთავარი გვერდი",
    "items_count": "{count} ნივთი ნაპოვნია",
    "email_subject": "კეთილი იყოს თქვენი მობრძანება, {name}!"
}
```

## Adding Languages Incrementally

You don't need to translate everything at once. Run `Create > Translation` per page, per language, as needed. The skill detects existing translation infrastructure on a page and extends it rather than duplicating work.

## Updating Translations

When you add new features to an already-translated page (via `Edit > Page`), new hardcoded strings may appear without translation keys. Run `Edit > Translation` to scan for and translate any newly introduced text.

## Removing a Language

Use `Delete > Translation` to remove a language's `lng.*.json` file. Note that this does not automatically strip the language from `<translation languages="...">` tags across pages - follow up with `Edit > Page` if you want that cleanup.
