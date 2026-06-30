# Edit > API

The `Edit > API` skill modifies an existing API file. Select the API file to edit and describe what needs to change - the AI rewrites it with full awareness of your current database schema and available PHP methods.

## Example Task

```
Add pagination support to v1.getRecipes.php - accept 'page' and 'per_page' 
GET parameters, return total count alongside the results
```

## Menu Visibility

This option is only shown when at least one API file exists in `Apis/`.
