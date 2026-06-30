# Edit > Translation

The `Edit > Translation` skill updates an existing translation - either correcting translation values in a `lng.*.json` file, adding new keys, or updating translated strings across multiple pages.

## Example Task

```
Update the Georgian translation file to translate the new task status labels 
we added to the dashboard page last session
```

## What It Updates

- Existing `Space/lng.{lang}.json` files
- HTML `translate` attributes on pages that have new untranslated text
- JavaScript and PHP `lng()` calls for new hardcoded strings
