# Cron Management

PHPShift manages cron job execution during local development through a Python background thread and provides the scaffolding needed for production cron setup. This page covers the operational side of working with crons day to day.

## Starting and Stopping Crons in Development

When you run `phpshift start` and cron files exist in `Crons/`, you're asked:

```
Want to start cron jobs?
```

If you confirm, a background Python thread starts, calling:

```bash
php cron -auto
```

once every 60 seconds for the lifetime of your session.

You can also control this manually from the skill menu at any time:

```
More > StartCrons   - Start the background cron thread
More > StopCrons    - Stop the background cron thread
```

The menu dynamically shows the relevant option (Start vs Stop) based on current thread state.

## How -auto Mode Works

The `cron` entry point script, when called with `-auto`, iterates every `.php` file in `Crons/` and executes each one. Each cron file contains its own internal time-check guard (e.g., `if (date('H:i') !== '00:00') exit;`), so calling all crons every minute is safe - only the ones whose schedule matches the current time actually perform work.

## Manual Single-Cron Execution

To test a specific cron job without waiting for its schedule or running the full auto cycle:

```bash
php cron sendDigest
```

This runs `Crons/sendDigest.php` directly, ignoring its internal time-check guard - useful for debugging.

## Cron Output and Logging

Cron jobs typically log their activity to files in `Space/`:

```php
file_put_contents(__DIR__ . '/../Space/digest.log', date('Y-m-d H:i:s') . " - Sent to " . count($users) . " users\n", FILE_APPEND);
```

Use `Read > Log` to have the AI analyze these logs and surface issues without manually scanning through raw text.

## Production Cron Setup

PHPShift's background thread is a **development convenience only** - it does not run when PHPShift's CLI is closed. For production, configure a real system cron job on your server:

```bash
# crontab -e
* * * * * php /var/www/myapp/cron -auto >> /var/www/myapp/Space/cron-system.log 2>&1
```

This mirrors the development behavior: called every minute, each job self-determines whether it's time to run.

## Common Cron Patterns

| Pattern | Guard |
|---------|-------|
| Every minute | *(no guard)* |
| Every 5 minutes | `if ((int)date('i') % 5 !== 0) exit;` |
| Hourly | `if (date('i') !== '00') exit;` |
| Daily at specific time | `if (date('H:i') !== '03:00') exit;` |
| Weekly on a day | `if (date('D H:i') !== 'Mon 08:00') exit;` |
| Monthly on a date | `if (date('d H:i') !== '01 00:00') exit;` |

## Debugging Cron Issues

1. Check `Space/*.log` files for output via `Read > Log`
2. Run the cron manually: `php cron {filename}`
3. Verify the database schema the cron depends on hasn't changed unexpectedly
4. Confirm `More > StartCrons` thread is actually running if testing live auto-execution
