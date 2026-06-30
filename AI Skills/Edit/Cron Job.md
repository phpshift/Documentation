# Edit > CronJob

The `Edit > CronJob` skill modifies an existing cron job file. Select the cron to edit and describe the required changes - the AI updates the schedule logic, database queries, or notification logic as needed.

## Example Task

```
Change the digest cron to run at 9 AM instead of midnight, 
and add a check to skip users who have unsubscribed from email notifications
```

## Menu Visibility

This option is only shown when at least one cron file exists in `Crons/`.
