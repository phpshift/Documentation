# Read > Log

The `Read > Log` skill performs AI-powered analysis of your application's log files. It reads the log content and your description of what you're looking for, then produces a structured `summary.md` highlighting key issues, errors and anomalies.

## How It Works

1. PHPShift lists all `*.log` files in `Space/`
2. You select the log file to analyze
3. You describe what you're looking for (or ask for a general summary)
4. The AI reads the log content and generates a `summary.md` report

## Example Tasks

```
Find all 500 errors from the last hour and tell me what caused them
```

```
Summarize all database errors in this log
```

```
Are there any repeated failed login attempts? Show me the patterns.
```

```
Give me a general health summary of this log file
```

## Generated Output

A `summary.md` report with:

- **Key events** - Critical errors, warnings and important operations
- **Error patterns** - Repeated issues grouped by type
- **Potential causes** - AI interpretation of what went wrong
- **Actionable insights** - Specific suggestions for investigation or fixes
- **Timeline** - Chronological highlights if timestamps are present

## Log File Location

Application log files should be written to `Space/` in your project. For example, a cron job might write to `Space/digest.log`. Your PHP code writes to these files using standard PHP file I/O or the `App::error()` logging utility.

## Menu Visibility

Only shown when at least one `.log` file exists in `Space/`.
