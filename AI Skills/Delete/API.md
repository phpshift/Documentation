# Delete > API

Removes an API file from `Apis/`.

## Process

1. PHPShift lists all existing API files
2. You select which one to remove
3. The `.php` file is deleted

## Note

This does not remove associated database tables. Use `Read > Database` for cleanup SQL if needed.

## Menu Visibility

Only shown when at least one API file exists in `Apis/`.
