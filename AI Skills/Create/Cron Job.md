# Create > CronJob

The `Create > CronJob` skill generates a PHP cron job script in `Crons/`. Cron jobs run on a schedule and handle background tasks independent of user requests.

## Context the AI Receives

- Current database schema and MySQL version
- All existing project file paths
- Available PHP methods from `.system/`

## Generated Output

A single PHP file in `Crons/` named in camelCase (e.g., `cleanExpiredSessions.php`). The file contains:

- A time-check guard at the top to control when it runs
- Business logic using the full framework DB ORM and App helpers
- Proper error logging

## Example Task

```
Every day at 3 AM, delete sessions from the sessions table that expired 
more than 7 days ago, and log the count of deleted records
```

Output: `Crons/cleanExpiredSessions.php`

```php
<?php

if (date('H:i') !== '03:00') exit;

$cutoff  = date('Y-m-d H:i:s', strtotime('-7 days'));
$deleted = DB::delete('sessions', 'expires_at < :cutoff', [':cutoff' => $cutoff]);

App::error("Cleaned expired sessions. Count: " . ($deleted ? 'success' : '0'));
```

## Schedule Examples

Describe your schedule in natural language - the AI translates it to PHP time-check logic:

| Description | Generated guard |
|-------------|----------------|
| Every minute | *(no guard - runs every invocation)* |
| Every hour | `if (date('i') !== '00') exit;` |
| Every day at midnight | `if (date('H:i') !== '00:00') exit;` |
| Every Monday at 8 AM | `if (date('D H:i') !== 'Mon 08:00') exit;` |
| First day of month | `if (date('d H:i') !== '01 00:00') exit;` |

## Composer Dependencies

If the cron job needs a package (e.g., `phpmailer/phpmailer` for email), PHPShift detects it from `config.json` and installs it before confirming.

## After Creation

The cron file is immediately executable. If the PHPShift cron thread is running (`More > StartCrons`), it will be picked up within the next minute cycle. You can also test it manually:

```bash
php cron sendDigest
```
