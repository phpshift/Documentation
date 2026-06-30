# Environment Variables

All PHPShift project configuration lives in the `.env` file at the project root. This file is never exposed publicly (`.htaccess` denies all access to it) and is excluded from Git via `.gitignore`.

## System Variables (Framework)

These are set during `phpshift new` and managed by PHPShift:

| Variable | Description | Example |
|----------|-------------|---------|
| `PROJECT_NAME` | Human-readable project name | `My Recipe App` |
| `PROJECT_AUTHOR` | Author name (used in LICENSE) | `Jane Doe` |
| `PROJECT_LOCAL` | Local development domain | `myapp.local` |
| `PROJECT_PRODUCTION` | Production domain | `myapp.com` |
| `PROJECT_LANDING` | Default landing page group/name | `public/welcome` |
| `PROJECT_KEY` | Auto-generated secure app key | *(random token)* |
| `PHPSHIFT_AIMODEL` | AI model identifier string | `claude-3-5-sonnet-20241022` |
| `PHPSHIFT_AIKEY` | AI API key or key file path | `sk-ant-...` |
| `DB_HOST` | MySQL host | `localhost` |
| `DB_NAME` | MySQL database name | `myapp_db` |
| `DB_USER` | MySQL username | `root` |
| `DB_PASS` | MySQL password | `secret` |
| `PRODUCTION` | Production mode flag (`1` or `0`) | `0` |
| `ALLOWED` | Comma-separated allowed IPs (dev restriction) | `127.0.0.1` |

## Project Variables (Your App)

Below the `## Project` comment section, PHPShift and the AI add custom variables as your project needs them. For example:

```
STRIPE_SECRET_KEY=""
SMTP_HOST=""
SMTP_PORT=""
SMTP_USER=""
SMTP_PASS=""
```

These are added automatically when generated PHP code contains `App::env('VARIABLE_NAME')` calls - PHPShift detects new variable names and appends them as empty entries, then opens the file in VS Code for you to fill in.

## Reading Variables in PHP

```php
$key = App::env('STRIPE_SECRET_KEY');
$host = App::env('SMTP_HOST');
```

## Reading Variables in Python (PHPShift CLI)

```python
model = Help.getEnv('PHPSHIFT_AIMODEL')
Help.setEnv('PHPSHIFT_AIMODEL', 'new-model-name')
```

## Production Mode

Set `PRODUCTION="1"` on your live server to enable production behaviors:

- Error messages are suppressed from public output
- Debug tracing is disabled
- The `ALLOWED` IP list is enforced (if set)

## Security

- The `.env` file is blocked by `.htaccess` with `Require all denied`
- It is listed in `.gitignore` to prevent accidental commits
- Never hardcode credentials in page PHP files - always use `App::env()`
