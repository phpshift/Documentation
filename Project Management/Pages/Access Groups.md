# Access Groups

Access groups are PHPShift's authorization system for pages. Every page belongs to a group and the group determines which users can access it.

## How Groups Work

The group is the first part of a page's folder name: `Pages/{group}.{name}/`. The framework checks the corresponding static method in the `.access` PHP class before serving any page:

```php
class Access
{
    public static function public()
    {
        // Everyone can access 'public' pages
        return true;
    }

    public static function private()
    {
        // Only logged-in users can access 'private' pages
        return App::isLoggedIn();
    }

    public static function admin()
    {
        // Only admin users
        return App::isAdmin() && App::isLoggedIn();
    }
}
```

If the access method returns `false`, the framework redirects or returns a 403 response.

## Default Groups

| Group | Typical Use |
|-------|------------|
| `public` | Pages accessible to all visitors (landing, login, signup) |
| `private` | Pages for authenticated users only (dashboard, profile) |
| `admin` | Pages for administrators only |
| `callback` | Webhook receiver endpoints (no standard auth) |

You can define any group name - the AI respects whatever name you use.

## Auto-Generated Group Methods

When the AI creates a page with a group that doesn't have an existing method in `.access`, PHPShift automatically runs the `Render > Group` skill. This skill:

1. Receives the new group name and a description of what the group means
2. Generates the appropriate static method in `.access`

For example, creating a page with group `premium` triggers generation of:

```php
public static function premium()
{
    // Premium means only subscribed users with active premium plan can access
    return App::isLoggedIn() && App::env('USER_PLAN') === 'premium';
}
```

The description used to generate this method comes from the `group-description` field in `config.json` that the AI populates when creating the page:

```json
{
    "page-group": "premium",
    "group-description": "premium - means that only subscribed users with an active premium plan can access the page"
}
```

## Checking Groups in Your PHP Code

Within page PHP code, you don't need to manually check group access - the framework handles it before your class is even instantiated. However, if you need to verify access inside a method (e.g., for an `App.call()` request), use the App helper:

```php
public function getPrivateData($post = [], $get = [], $files = [])
{
    if (!App::isLoggedIn()) {
        return App::failed('Unauthorized');
    }
    // ...
}
```

## Editing the Access Class

The `Edit > Access` skill lets you modify the `.access` file using AI. Describe what you need ("add a moderator group that requires the user to have role='mod' in the database") and PHPShift updates the class while preserving existing group methods.
