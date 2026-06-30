# APIs Overview

PHPShift supports standalone REST API files for endpoints that operate independently of pages - webhooks, mobile app backends, external integrations, and internal system services.

## When to Use APIs vs Page Methods

**Use `App.call()` page methods** when:
- The front-end JavaScript on the same page is the caller
- The interaction is tightly coupled to a specific page
- You're building CRUD operations for that page's data

**Use API files** when:
- The caller is external (mobile app, third-party service, webhook)
- Multiple pages or services need the same endpoint
- You need versioned API routes (v1, v2)
- You're building a public API with token authentication

## API File Location and Naming

API files live in `Apis/` and follow strict naming:

```
Apis/
├── v1.getItems.php
├── v1.createItem.php
├── v2.getItems.php
└── v1.processWebhook.php
```

- Version prefix: `v1`, `v2`, etc.
- Name: camelCase
- Full name format: `{version}.{camelCaseName}.php`

## API URL Routing

The framework routes API calls automatically. An API file `v1.getItems.php` is accessible at:

```
GET /api/v1/getItems
```

The router parses the version and name from the URL and dispatches to the correct file.

## Access Categories

Each API file declares its category (access control) and allowed HTTP method on the first line:

```php
<?php category('public', 'GET');
```

| Category | Who Can Call |
|----------|-------------|
| `public` | Anyone without authentication |
| `callback` | Webhook/callback source with verification |
| `systemApi` | Internal system (server-to-server) |
| `userApi` | Authenticated users with valid API token |
| `userWebhook` | Authenticated user webhook with token |

## Response Format

All API responses use the same JSON envelope as page methods:

```json
{
    "message": "Items retrieved successfully",
    "response": [...],
    "error": false
}
```

Use `App::done()`, `App::failed()`, or `App::info()` to produce these - never `echo json_encode()` directly.

## Managing APIs

| Action | Skill |
|--------|-------|
| Create a new API | `Create > API` |
| Edit an existing API | `Edit > API` |
| Delete an API | `Delete > API` |

PHPShift's skill menu dynamically shows or hides Edit/Delete API options depending on whether any API files currently exist in `Apis/`.
