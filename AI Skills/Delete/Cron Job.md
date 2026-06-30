# Delete > CronJob

Removes a cron job file from `Crons/`.

## Process

1. PHPShift lists all existing cron files
2. You select which one to remove
3. The `.php` file is deleted

If the cron thread is running, the removed job will simply not be found on the next execution cycle.

## Menu Visibility

Only shown when at least one cron file exists in `Crons/`.
