# Create > API

The `Create > API` skill generates one or more PHP REST API files in `Apis/`. Use it when you need versioned endpoints callable from external clients (mobile apps, third-party services, webhooks).

## Context the AI Receives

- Current database schema and MySQL version
- All existing file paths (pages, APIs, crons)
- All available PHP methods from `.system/`

## Generated Output

The AI produces one or more PHP files in the format `v{version}.{camelCaseName}.php`, each containing a properly structured API class.

## Example Task

```
Create a v1 public API endpoint that returns a paginated list of public recipes, 
supporting optional category filter and keyword search via GET parameters
```

Output: `Apis/v1.getRecipes.php`

```php
<?php category('public', 'GET');

class ApiGetRecipes
{
    public function init($json = [], $post = [], $get = [])
    {
        $page     = max(1, (int)($get['page'] ?? 1));
        $category = trim($get['category'] ?? '');
        $keyword  = trim($get['keyword'] ?? '');
        $limit    = 20;
        $offset   = ($page - 1) * $limit;

        $where  = 'r.public = 1';
        $params = [];

        if ($category) {
            $where .= ' AND r.category = :cat';
            $params[':cat'] = $category;
        }
        if ($keyword) {
            $where .= ' AND r.title LIKE :kw';
            $params[':kw'] = '%' . $keyword . '%';
        }

        $params[':limit']  = $limit;
        $params[':offset'] = $offset;

        $recipes = DB::query(
            "SELECT r.id, r.title, r.category, r.created 
             FROM recipes r WHERE {$where} 
             ORDER BY r.created DESC LIMIT :limit OFFSET :offset",
            $params
        );

        return App::done('Recipes retrieved', ['recipes' => $recipes, 'page' => $page]);
    }
}
```

## Versioning

Always specify the version in your task description. The AI uses it as the file version prefix. When creating a v2 update to an existing endpoint, the original v1 file is untouched.

## Composer Dependencies

If the generated API requires a Composer package, PHPShift detects it from `config.json` and prompts you to install it before confirming the skill run.
