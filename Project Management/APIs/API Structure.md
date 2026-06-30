# API Structure

Every PHPShift API file follows a strict structure enforced by AI generation rules and the framework's dispatch engine.

## Full API File Example

```php
// Specify API category and allowed HTTP method
<?php category('userApi', 'POST');

/**
 * Required parameters:
 * - $json['item_id'] (int) - ID of item to update
 * - $json['title']   (string) - New item title
 */

// Class name: 'Api' prefix + PascalCase name
class ApiUpdateItem
{
    // Entry point - always 'init', always these three arguments
    public function init($json = [], $post = [], $get = [])
    {
        $itemId = (int)($json['item_id'] ?? 0);
        $title  = trim($json['title'] ?? '');

        if (!$itemId || !$title) {
            return App::failed('Invalid parameters');
        }

        $updated = DB::update('items', ['title' => $title], 'id = :id', [':id' => $itemId]);
        if (!$updated) {
            return App::failed('Update failed');
        }

        return App::done('Item updated', ['item_id' => $itemId]);
    }

    private function validateOwnership($itemId, $userId)
    {
        $item = DB::query('SELECT user FROM items WHERE id = :id', [':id' => $itemId]);
        return !empty($item) && $item[0]['user'] === $userId;
    }
}
```

## Naming Rules

| Element | Rule | Example |
|---------|------|---------|
| File name | `{version}.{camelCase}.php` | `v1.updateItem.php` |
| Class name | `Api` + PascalCase of name | `ApiUpdateItem` |
| Entry method | Always `init` | `public function init(...)` |
| Helper methods | `private` | `private function validateOwnership(...)` |

## Parameter Sources

The `init` method receives three arguments:

- `$json` - JSON body (for POST/PUT requests with `Content-Type: application/json`)
- `$post` - Form POST fields
- `$get` - URL query string parameters

Always validate and sanitize all inputs before using them.

## Return Methods

| Method | Use When |
|--------|---------|
| `App::done($msg, $data)` | Success with data |
| `App::failed($msg)` | Business logic failure (not a server error) |
| `App::info($msg, $data)` | Informational response |
| `App::error($msg)` | Log a developer-facing error (does not return to caller) |

## Calling APIs from External Clients

From a mobile app or external service, call the API endpoint directly:

```
POST https://myapp.com/api/v1/updateItem
Content-Type: application/json
X-System: {api_token}

{
    "item_id": 42,
    "title": "New Title"
}
```

The `X-System` header is used for API token authentication in `userApi` and `systemApi` categories. The framework's token validation is handled automatically before your `init()` method is called.

## Calling APIs from Within the Project's JavaScript

For API endpoints that your own front-end JavaScript calls, you can use `App.call()` pointing to the API path:

```javascript
App.call('api/v1/updateItem', 'init', {item_id: 42, title: 'New Title'}, (echo) => {
    if (!echo.error) console.log('Updated!');
});
```

However, for page-level interactions, generating a page method (via `Create > Page` or `Edit > Page`) is preferred over separate API files.
