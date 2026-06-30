# Git Integration

PHPShift treats Git as a first-class part of the development workflow. Every confirmed skill run offers an immediate commit opportunity, keeping your history granular and meaningful without requiring you to leave the terminal.

## Git Initialization

During `phpshift new`, you're asked whether to initialize a Git repository. If yes, PHPShift runs `git init` and creates an initial `.gitignore` that excludes:

```
.env
.system/vendor/
Space/*.log
*.local
node_modules/
```

## Commit Prompt After Every Skill

When you confirm a skill's output (select **Yes**), PHPShift immediately asks:

```
Want to commit these changes?
```

If you confirm, you're prompted for a commit message - PHPShift can suggest one based on the skill and your task description, or you can write your own.

```bash
git add .
git commit -m "Add user dashboard page with task stats"
```

## Push Prompt

After committing, if a remote is configured, PHPShift asks:

```
Want to push to remote?
```

Confirming runs `git push` against the current branch's tracked remote.

## Why Commit After Every Skill

This pattern produces a clean, readable commit history where each commit corresponds to one logical AI-generated change - a new page, an edit, a translation addition. This makes it easy to:

- Review what the AI changed in any single commit
- Revert a specific feature without affecting others
- Understand project history without digging through large, mixed commits

## Using Read > Git for Everything Else

The automatic commit/push flow handles the common case. For everything else - branching, merging, diffing, resetting, configuring remotes - use the `Read > Git` skill, which translates natural language into the appropriate Git command. See **AI Skills → Read → Git** for details.

## Recommended Branching Strategy

While PHPShift doesn't enforce a specific Git workflow, a simple effective pattern for AI-paired development is:

1. Work on `main` for small, low-risk changes (the patch system already protects you from bad generations)
2. Use `Read > Git` to create a feature branch for larger multi-skill features
3. Merge back to `main` once the feature is complete and tested

## .gitignore Maintenance

If your generated code introduces new sensitive files or build artifacts, update `.gitignore` manually or via `Read > Git`:

```
Add a .gitignore rule to exclude the new uploads/ directory
```
