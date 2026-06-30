# PHP Layer

The PHP layer is the server-side runtime for every PHPShift project. It is driven by a framework engine inside `.system/` that handles routing, access control, database ORM, API dispatch, and cron execution. Generated code lives on top of this engine.

## Page Classes

Every page has a corresponding PHP class in its `code.php` file. Classes follow a strict naming convention:

- Page name: `edititem` → Class name: `PageEditItem`
- Page name: `dashboard` → Class name: `PageDashboard`

The class is a plain PHP class with public methods. Each public method is directly callable from the front-end via JavaScript's `App.call()`:

```php
class PageDashboard
{
    public function getStats($post = [], $get = [], $files = [])
    {
        $stats = DB::query("SELECT COUNT(*) as total FROM tasks WHERE user = :uid", [
            ':uid' => App::env('CURRENT_USER_ID')
        ]);
        return App::done('Stats loaded', ['stats' => $stats]);
    }

    private function validateAccess($userId)
    {
        // Private helper - not callable from front-end
    }
}
```

Method arguments are always `($post = [], $get = [], $files = [])` - populated automatically by the framework from the incoming HTTP request.

## API Files

API files live in `Apis/` and follow a similar pattern, but declare their category and allowed HTTP method at the top:

```php
<?php category('public', 'GET');

class ApiGetItems
{
    public function init($json = [], $post = [], $get = [])
    {
        // API entry point - always named 'init'
        return App::done('Items retrieved', ['items' => []]);
    }
}
```

Available API categories:
- `public` - No authentication required
- `callback` - Webhook/callback endpoint
- `systemApi` - Internal system access
- `userApi` - Authenticated user access
- `userWebhook` - Authenticated user webhook

## Cron Job Files

Cron files in `Crons/` are PHP scripts executed by the PHPShift cron thread once per minute (or per schedule defined inside the file):

```php
<?php

// Run at midnight
if (date('H:i') !== '00:00') exit;

// Cleanup expired sessions
DB::delete('sessions', 'expires_at < NOW()');
```

PHPShift manages cron execution through a Python background thread that calls `php cron -auto` every minute. You can start and stop cron processing from inside the skill menu under `More > StartCrons` and `More > StopCrons`.

## The App Helper Class

`App` is a global PHP static utility class provided by the framework engine. Generated code uses it for all framework interactions:

| Method | Purpose |
|--------|---------|
| `App::env($key)` | Read `.env` variable |
| `App::done($msg, $data)` | Return `{"message": ..., "response": ..., "error": false}` |
| `App::failed($msg)` | Return `{"message": ..., "error": true}` |
| `App::info($msg, $data)` | Return informational response |
| `App::error($msg)` | Log a developer error |
| `App::lng($key, $text, $vars)` | Return translated string |

All API responses follow the same JSON envelope so the front-end `App.call()` handler always knows what to expect.

## Composer Plugins

Generated PHP code can require Composer packages. When PHPShift's AI detects a needed package (e.g., `phpmailer/phpmailer`), it includes it in `config.json` under `composer-plugins`. PHPShift then prompts you to confirm before running:

```bash
composer require phpmailer/phpmailer
```

The `vendor/` directory and `composer.json` are both patch-tracked, so plugin installation can be rolled back if needed.

## Environment Variables

All sensitive configuration is stored in `.env` and read via `App::env()`. Generated code uses this pattern to avoid hardcoding credentials:

```php
$apiKey = App::env('STRIPE_API_KEY');
```

When the AI detects an `App::env()` call referencing a variable that doesn't exist in `.env` yet, PHPShift adds it automatically with an empty value and opens the file in VS Code for you to fill in.
