# Delete > Translation

Removes a language translation JSON file from `Space/`.

## What Gets Removed

The `Space/lng.{lang}.json` file for the selected language. Translation tag references in HTML pages are not automatically cleaned up - you may want to run `Edit > Page` on affected pages to remove the unused language from `<translation languages="...">` tags.

## Menu Visibility

Only shown when at least one `lng.*.json` file exists in `Space/`.
