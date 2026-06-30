# Read > Git

The `Read > Git` skill translates natural language into Git CLI commands and executes them in your project directory. It handles any Git operation - from inspecting history to managing branches to configuring remotes.

## How It Works

1. You describe the Git operation you need
2. The AI generates a single, ready-to-execute `command.sh` containing the appropriate Git command
3. PHPShift runs it in your project directory and shows the output

## Example Tasks

```
Show me the last 10 commits with their dates and messages
```

```
Create a new branch called feature/user-auth and switch to it
```

```
Show all files changed in the last commit
```

```
Revert the most recent commit but keep the file changes staged
```

```
Show the diff between main and the current branch
```

## Generated Output

A single `command.sh` file containing one Git command:

```bash
git log --oneline --format="%h %ad %s" --date=short -10
```

## Note

PHPShift's main workflow already handles `git add`, `git commit`, and `git push` natively after every skill confirmation. Use `Read > Git` for everything else - branching, diffing, inspecting history, resetting, configuring remotes, etc.

The generated command runs in your project root (`Help.cwd`). Commands requiring credentials (push, pull, clone with auth) assume your Git config or SSH keys are already set up.
