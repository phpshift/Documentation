# Edit > Access

The `Edit > Access` skill modifies the `.access` PHP class - the file that defines page access group authorization methods.

## What It Does

The AI receives the current `.access` content and the edit request, then generates an updated version of the file that adds, modifies, or removes group methods.

## Example Tasks

```
Add a 'moderator' group that requires the user to have role='mod' in the users table
```

```
Update the 'premium' group check to also verify the subscription hasn't expired
```

## Important Note

Every static method in `.access` corresponds to an access group used by pages in `Pages/{group}.{name}/`. Removing a group method will break all pages that use that group. PHPShift does not automatically check for this - review your page folders before deleting groups.
