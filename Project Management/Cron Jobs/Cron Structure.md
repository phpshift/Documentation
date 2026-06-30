# Cron Job Structure

PHPShift cron files are PHP scripts that run autonomously. They have direct access to the full framework - database ORM, App helpers, environment variables - just like page PHP classes.

## Basic Cron File

```php
<?php

// Only run once per day at midnight
if (date('H:i') !== '00:00') exit;

// Clean up expired API tokens
$deleted = DB::delete('api', 'expires < :now AND expires > 0', [':now' => time()]);

if ($deleted) {
    App::error("Cleaned " . count($deleted) . " expired API tokens");
}
```

## Cron File with Notifications

```php
<?php

// Run every hour at :00
if (date('i') !== '00') exit;

// Get users with pending notifications
$users = DB::query(
    'SELECT u.email, COUNT(n.id) as count FROM notifications n 
     JOIN users u ON u.id = n.user 
     WHERE n.sent = 0 AND n.scheduled <= NOW() 
     GROUP BY u.id',
    []
);

foreach ($users as $user) {
    // Send notification email
    $mailer = new PHPMailer\PHPMailer\PHPMailer();
    // ... configure and send
    
    // Mark as sent
    DB::update('notifications', ['sent' => 1], 'user = :uid AND sent = 0', [
        ':uid' => $user['id']
    ]);
}
```

## Naming Rules

| Element | Rule | Example |
|---------|------|---------|
| File name | camelCase `.php` | `sendDigest.php` |
| No class required | Plain script | - |
| Schedule logic | Internal `if` check | `if (date('H:i') !== '00:00') exit;` |

Unlike pages and APIs, cron files are not classes. They are plain PHP scripts executed directly by the cron runner.

## Available Resources

Cron files have access to everything the framework provides:

```php
// Read environment variables
$apiKey = App::env('EXTERNAL_API_KEY');

// Database queries
$records = DB::query('SELECT * FROM items WHERE ...', [...]);
DB::insert('logs', ['action' => 'cron_ran', 'time' => date('Y-m-d H:i:s')]);

// Translations (if needed for notification text)
$subject = App::lng('digest_subject', 'Your daily digest');

// Error logging
App::error('Something went wrong in cron: ' . $errorMessage);
```

## Generating Cron Jobs with AI

The `Create > CronJob` skill generates a fully functional cron file. Describe what the job should do and when:

```
Every day at 2 AM, find all users who have not logged in for 30 days 
and send them a re-engagement email using PHPMailer
```

PHPShift provides the AI with your current database schema and available PHP methods, so generated cron code correctly references your actual table names and columns.

After generation, if the cron needs a Composer plugin (like PHPMailer), PHPShift installs it immediately.

## Viewing Cron Logs

Cron jobs can write to log files in `Space/`. Use the `Read > Log` skill to have the AI analyze a log file and summarize errors or anomalies.
