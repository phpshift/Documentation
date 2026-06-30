# Development Loop

PHPShift's core workflow is a tight feedback loop: describe what you want, review the AI's output, confirm or retry. Understanding this loop helps you work efficiently and avoid frustration when output isn't quite right the first time.

## The Loop

```
1. Select a skill from the menu
2. Describe your task in plain language
3. PHPShift collects project context and sends it to the AI
4. AI generates structured file output
5. Files are written, DB changes staged
6. PHPShift shows you the result
7. You choose: Yes / Redo / No
```

## The Three Outcomes

### Yes - Keep Changes

The files remain on disk, database changes are committed (`DB.clear()`), and you're offered a Git commit prompt. This finalizes the skill run.

### Redo - Adjust and Retry

All changes roll back (files restored to pre-skill state, database reverted via `DB.rollback()`). PHPShift asks you to refine your description:

```
What would you like to change about your request?
```

You can add clarifying detail, correct a misunderstanding, or specify what was wrong with the previous attempt. The skill runs again with your updated description - often the AI gets it right on the second pass because it now has more specific guidance.

### No - Discard Entirely

All changes roll back completely, and you return to the main skill menu. No trace of the attempt remains.

## Writing Effective Task Descriptions

The quality of AI output is directly tied to the quality of your description. Effective descriptions include:

- **What the feature does** - the functional requirement
- **Who can access it** - which access group, if relevant
- **Data involved** - what gets stored, read, or modified
- **Edge cases** - validation rules, error states
- **Style references** - "match the dashboard page's design"

### Weak Example
```
Add a login page
```

### Strong Example
```
Create a public login page with email and password fields, client-side 
validation for email format, a "remember me" checkbox, and a link to 
a password reset flow. On successful login, redirect to /dashboard. 
On failure, show an inline error message without page reload. 
Match the styling of the existing welcome page.
```

## Iterating Across Multiple Skill Runs

Complex features often require multiple skill invocations:

```
1. Create > Page (basic structure)
2. Edit > Page (add specific interactive feature)
3. Edit > Page (refine styling)
4. Create > Translation (add language support)
```

Each run benefits from full project context, so the AI stays consistent across iterations even when you're building incrementally.

## When to Start Fresh vs. Edit

If a page has accumulated many small edits and feels inconsistent, consider `Render > Page` to regenerate it cleanly from its `readme.md` description, rather than continuing to patch with `Edit > Page`.
