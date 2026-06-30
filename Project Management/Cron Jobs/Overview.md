# Cron Jobs Overview

PHPShift includes a built-in cron job management system that runs scheduled PHP scripts in the background during your development session - and on your production server via a standard system cron.

## What Cron Jobs Are For

Cron jobs are background tasks that run on a schedule, independent of user requests. Common uses in PHPShift projects:

- Sending scheduled email digests or notifications
- Cleaning up expired sessions, tokens, or temporary files
- Syncing data from external APIs
- Generating reports or aggregating statistics
- Processing queued background jobs

## Cron File Location

Cron files live in `Crons/`:

```
Crons/
├── sendDigest.php
├── cleanExpired.php
└── syncPrices.php
```

## How PHPShift Executes Crons

During a development session (`phpshift start`), if cron files exist, PHPShift asks "Want to start cron jobs?" A Python background thread is started that fires:

```bash
php cron -auto
```

once every minute. The `cron` entry point file (at project root) reads all files in `Crons/`, determines which ones should run based on their internal time checks and executes them.

You can manually control cron execution from the skill menu:

- `More > StartCrons` - Start the background cron thread
- `More > StopCrons` - Stop the background cron thread

## Production Cron Setup

On your production server, set up a system cron to call the same entry point:

```bash
* * * * * php /var/www/myapp/cron -auto
```

The cron files themselves contain internal schedule logic (e.g., only run at midnight), so running `cron -auto` every minute is safe - each job self-determines whether it's time to execute.

## Managing Cron Jobs

| Action | Skill |
|--------|-------|
| Create a new cron job | `Create > CronJob` |
| Edit an existing cron job | `Edit > CronJob` |
| Delete a cron job | `Delete > CronJob` |

The skill menu shows Edit/Delete Cron options only when cron files exist in `Crons/`.
