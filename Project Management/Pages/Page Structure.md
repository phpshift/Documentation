# Page Structure

Understanding the internal structure of a PHPShift page helps you navigate generated code, make manual adjustments, and know how the PHP and JavaScript layers communicate.

## HTML (`page.html`)

The HTML file contains only the body content of the page - no `<!DOCTYPE html>`, `<html>`, `<head>`, or `<body>` tags. The framework injects those automatically along with:

- jQuery CDN link
- jQook CDN link
- Page-specific `style.css`
- Page-specific `script.js`
- Global `.assets/style.css`
- Global `.assets/script.js`
- SEO metadata from `Space/seo.html`

Images reference `/assets/default.png`, videos `/assets/default.mp4`, audio `/assets/default.mp3`.

Font Awesome free icons are available in HTML (the framework includes them).

Example HTML structure:

```html
<div class="hero-section">
  <h1 translate="hero_title">Welcome to MyApp</h1>
  <p translate="hero_subtitle">The fastest way to manage your tasks.</p>
  <button id="cta-btn">Get Started</button>
</div>
```

The `translate` attributes are added by the `Create > Translation` skill to mark strings for multilingual support.

## CSS (`style.css`)

Styles are scoped to the page and generated to match existing pages' design language. When creating a new page, PHPShift asks which existing page to use as a **styling example** - the AI receives that page's CSS and replicates its design system.

Generated CSS is fully responsive and avoids inline styles.

## JavaScript (`script.js`)

Page JavaScript uses jQuery and the global `App` JavaScript object. The key communication method is `App.call()`:

```javascript
App.call('dashboard', 'getStats', {}, (echo) => {
    if (echo.error) {
        App.alert(echo.message, 'error');
        return;
    }
    $('#total-tasks').text(echo.response.stats[0].total);
});
```

`App.call(url, method, data, callback)`:
- `url` - page name (maps to `Pages/public.{url}/`)
- `method` - public method name in the page's PHP class
- `data` - object sent as POST body
- `callback` - receives `echo` object: `{message, response, error}`

Translation strings in JS use `App.lng()`:

```javascript
const label = App.lng('save_button', 'Save Changes');
```

## PHP (`code.php`)

The PHP class handles all server-side logic. It is named with the page name in PascalCase with the `Page` prefix:

```php
class PageDashboard
{
    // Called via App.call('dashboard', 'getStats', {...}, callback)
    public function getStats($post = [], $get = [], $files = [])
    {
        // Validate inputs, query database, return response
        return App::done('Stats loaded', ['count' => 42]);
    }

    private function validateUser($userId)
    {
        // Private helpers are not callable from JS
    }
}
```

Rules enforced by the AI:
- All JS-callable methods are `public`
- Arguments are always `($post = [], $get = [], $files = [])`
- Input validation is always included
- Environment variables are read via `App::env('KEY_NAME')`
- Redirects are never done in PHP - always via JavaScript

## Front-End / Back-End Communication Flow

```
User action in browser
    ↓
script.js: App.call('pagename', 'methodName', data, callback)
    ↓
Framework routes POST to Pages/group.pagename/code.php → PagePagename::methodName()
    ↓
PHP processes → returns App::done(message, response)
    ↓
callback(echo) in script.js receives { message, response, error }
    ↓
JS updates the DOM
```

No separate REST endpoints are needed for page-level interactions - the `App.call()` system handles PHP method dispatch transparently.
