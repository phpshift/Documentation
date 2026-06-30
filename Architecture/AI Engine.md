# AI Engine

The AI engine in PHPShift is the `aisi` Python module - a purpose-built skill runner that assembles context-rich prompts and communicates with AI models to generate structured file output.

## How Skills Work

Each skill is a folder containing two files:

- `prompt.md` - The AI prompt template with `[[placeholder]]` injection points
- `skill.py` - The Python skill runner class that controls execution flow

Some skills also have a `readme.md` describing the skill's purpose for developers.

### Prompt Templates

Prompt files use double-bracket placeholders that are replaced with live project data before the prompt is sent to the AI:

```
[[MESSAGE]]          → The developer's natural language task description
[[databaseSchema]]   → Current MySQL schema (tables, columns, types)
[[databaseVersion]]  → MySQL server version string
[[files]]            → List of all Pages/, Apis/, Crons/ file paths
[[phpMethods]]       → All PHP public static methods tagged with (AI-USE)
[[jsMethods]]        → All JavaScript App object methods tagged with (AI-USE)
[[styling]]          → CSS from a developer-selected existing page
[[pages]]            → readme.md content from all existing pages
[[codeBase]]         → Full file contents of a target page
[[logContent]]       → Content of a selected log file
[[frameworkStructure]] → .system/ PHP framework directory listing
[[frameworkCodebase]]  → Core framework PHP file contents
[[frameworkSolvedIssues]] → Resolved issue notes from solved.md
```

These placeholders are populated by the `Collectors` class at runtime, ensuring the AI always has accurate, current information.

### The Collectors Class

`Collectors` is a Python class in `.system/collectors.py` where each method corresponds to a named placeholder. The `aisi` module calls the appropriate collector method based on which placeholders appear in the skill's prompt.

For example, `[[phpMethods]]` triggers `Collectors.phpMethods()`, which:

1. Walks every `.php` file in `.system/`
2. Finds methods with `/** (AI-USE)` doc comments
3. Extracts method signatures and documentation
4. Returns a formatted PHP code block

This ensures the AI knows exactly which methods exist and how to use them - without you having to document anything extra.

## AI Response Parsing

The AI responds with named file blocks in markdown format:

````
[readme.md]
```md
## Goal
Create a user registration page...
```

[page.html]
```html
<div class="register-form">
  ...
</div>
```

[code.php]
```php
class PageRegister {
  public function register($post = [], ...) {
    ...
  }
}
```
````

`aisi` parses these blocks, extracts each filename and content pair, and writes them to disk. Binary or special files are handled accordingly.

## Model Selection

PHPShift supports multiple AI providers through the `AISI.models()` selector. During `phpshift new` or `More > ChangeAIModel`, you pick a model from an interactive list. The model identifier and API key are stored in `.env`:

```
PHPSHIFT_AIMODEL="claude-3-5-sonnet-20241022"
PHPSHIFT_AIKEY="sk-ant-..."
```

For Google Vertex AI, the `PHPSHIFT_AIKEY` value is a relative path to a service account JSON file:

```
PHPSHIFT_AIMODEL="vertex/gemini-1.5-pro"
PHPSHIFT_AIKEY="credentials/service-account.json"
```

## Skill Execution Flow

```
Developer selects skill + types task description
         ↓
AISI loads prompt.md for the selected skill
         ↓
AISI calls Collectors for each [[placeholder]] found
         ↓
Full prompt assembled with live project context
         ↓
Prompt sent to AI model (via configured API)
         ↓
AI returns structured file blocks
         ↓
AISI parses response, writes files to disk
         ↓
PHPShift shows result, prompts for confirmation
```

## Skill Discovery

The main CLI menu is built dynamically by scanning the `Create/`, `Edit/`, `Delete/`, `Read/`, `Render/`, `More/`, and `PHPShift/` directories for `skill.py` files. The menu structure mirrors the directory tree, and options are filtered based on project state (e.g., "Edit API" is hidden if no APIs exist yet).
