# Framework Structure

A PHPShift project has a well-defined directory layout. Understanding this structure helps you navigate the generated codebase and know what each part is responsible for.

## Top-Level Directory Layout

```
my-project/
├── .assets/              # Public static assets served by Apache
│   ├── script.js         # Global jQook/jQuery app logic
│   ├── style.css         # Global base styles
│   ├── logo.png          # Project logo
│   ├── default.png       # Default image placeholder
│   ├── default.mp4       # Default video placeholder
│   ├── default.mp3       # Default audio placeholder
│   ├── robots.txt        # Search engine crawl rules
│   ├── sitemap.xml       # Auto-updated sitemap
│   └── background.webp   # Background asset
├── .system/              # PHP framework engine (internal, not public)
│   ├── app.php           # Router entry point (all requests route here via .htaccess)
│   ├── composer.json     # Composer dependency manifest
│   └── vendor/           # Composer-installed PHP packages
├── Apis/                 # PHP REST API files
│   └── v1.example.php
├── Crons/                # PHP cron job scripts
│   └── updateItems.php
├── Pages/                # Full-stack page units
│   ├── public.welcome/
│   │   ├── readme.md     # Page documentation
│   │   ├── page.html     # HTML structure
│   │   ├── style.css     # Page styles
│   │   ├── script.js     # Page JavaScript
│   │   └── code.php      # PHP business logic class
│   └── private.dashboard/
│       └── ...
├── Space/                # Project-level data files
│   ├── seo.html          # SEO meta tags (Open Graph, Twitter Card, etc.)
│   ├── lng.en.json       # English translation keys (auto-generated)
│   ├── lng.ka.json       # Georgian translation keys
│   └── *.log             # Application log files
├── .access               # PHP Access class - defines page access groups
├── .env                  # Environment configuration (never public)
├── .htaccess             # Apache routing, security headers, rate limiting
├── .gitignore            # Git ignore rules
├── .md                   # AI-readable project briefing (hidden file)
├── README.md             # Project readme (AI-generated or manual)
├── LICENSE               # Project license file
└── cron                  # PHP cron runner entry point
```

## The `.system/` Directory

This is the internal PHP framework engine. It is not editable by AI generation - it is the stable base that powers routing, authentication, database ORM, API dispatch, cron execution, and the `App` helper class.

Key PHP method families available to generated code:

- `App::env($key)` - Read environment variable
- `App::done($message, $data)` - Return successful JSON response
- `App::failed($message)` - Return error JSON response
- `App::info($message, $data)` - Return informational JSON response
- `App::error($message)` - Log internal developer error
- `App::lng($key, $text, $vars)` - Translate a string
- `App::redirect($url)` - Not used in generated code (handled by JS instead)

All custom PHP classes in pages and APIs extend this surface through static method calls.

## The Page Unit Structure

Each page lives in a folder named `{group}.{name}` inside `Pages/`. For example, a public dashboard page lives at `Pages/public.dashboard/`.

The **group** determines access control. The `.access` PHP class defines static methods named after groups:

```php
class Access {
    public static function private() {
        // Return true if current user passes the 'private' group check
    }
}
```

PHPShift generates group methods automatically if a new group is detected during page creation.

## URL Routing

All HTTP requests are routed through `.htaccess` to `.system/app.php`. The router maps:

- `/welcome` → `Pages/public.welcome/code.php` (PageWelcome class)
- `/dashboard` → `Pages/private.dashboard/code.php` (PageDashboard class)
- `/assets/*` → `.assets/*` (static files)
- API calls via `App.call(url, method, data, callback)` in JS → `Apis/` PHP files
