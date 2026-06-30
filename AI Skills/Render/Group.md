# Render > Group

The `Render > Group` skill generates a new static method in the `.access` PHP class for a page access group that doesn't exist yet.

## When It's Called

This skill is **usually called automatically** by PHPShift - not manually. When `Create > Page` or `Render > Page` produces a `config.json` specifying a group that has no method in `.access`, PHPShift automatically runs `Render > Group` before finishing.

You can also run it manually if you want to pre-define an access group before creating pages that use it.

## How It Works

PHPShift passes the group name and its `group-description` from `config.json` to the AI. The AI generates a new `public static function {groupName}()` method and appends it to the `.access` class.

## Example

If `config.json` contains:

```json
{
    "page-group": "subscriber",
    "group-description": "subscriber - means the user has an active paid subscription"
}
```

The generated method might be:

```php
public static function subscriber()
{
    if (!App::isLoggedIn()) return false;
    $user = DB::query('SELECT plan FROM users WHERE id = :id', [':id' => App::userId()]);
    return !empty($user) && $user[0]['plan'] === 'subscriber';
}
```

## Manual Use

Run `Render > Group` manually when you want to:

- Define a new access group before any pages use it
- Preview what the AI would generate for a group before committing to it
- Update the group logic via `Edit > Access` reference

## Note

The `.access` class is PHP. The static method names must match the group folder prefix exactly (case-insensitive matching may vary by server). Keep group names lowercase and simple.
