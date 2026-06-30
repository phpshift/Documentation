# Rate Limiting

PHPShift's framework engine includes built-in rate limiting at two levels: per-session (ID rate) and per-IP (IP rate). These are configured in `.htaccess` via environment variables read by `.system/app.php`.

## Configuration Variables

```apache
# Request body size limit (MB)
SetEnv CONTENT_LENGTH 2

# Session lock duration after rate limit hit (seconds)
SetEnv LOCK_TIME 30

# Per-session rate: time window (seconds)
SetEnv IDR_PERIOD 10

# Per-session rate: max requests per window
SetEnv IDR_AMOUNT 10

# Per-IP rate: time window (seconds)
SetEnv IPR_PERIOD 60

# Per-IP rate: max requests per window
SetEnv IPR_AMOUNT 200
```

## How It Works

### ID Rate (Session-Level)

Tracks requests per session identifier. If a single user session makes more than `IDR_AMOUNT` requests within `IDR_PERIOD` seconds, the session is locked for `LOCK_TIME` seconds.

**Default:** max 10 requests per 10 seconds per session. Locks for 30 seconds on violation.

Use case: protects against AJAX-heavy pages accidentally hammering the server and catches scripted abuse using a stolen session.

### IP Rate (IP-Level)

Tracks requests per client IP address. If a single IP makes more than `IPR_AMOUNT` requests within `IPR_PERIOD` seconds, further requests from that IP are temporarily blocked.

**Default:** max 200 requests per 60 seconds per IP.

Use case: protects against scraping, brute force login attempts and DDoS from a single source.

## Tuning for Your Application

The defaults are conservative. For applications with heavy front-end JavaScript activity (many `App.call()` requests per page), you may need to increase `IDR_AMOUNT` or `IDR_PERIOD`:

```apache
# Allow more requests for a data-heavy dashboard
SetEnv IDR_PERIOD 10
SetEnv IDR_AMOUNT 30
```

For public APIs that expect high volume from legitimate clients:

```apache
SetEnv IPR_PERIOD 60
SetEnv IPR_AMOUNT 1000
```

## LOCK_TIME

When a session hits the ID rate limit, it is locked for `LOCK_TIME` seconds. Legitimate users experience a brief pause before they can make requests again. Scripted attackers face a compounding delay.

## File Upload Size

`CONTENT_LENGTH` sets the maximum allowed request body in megabytes. The framework rejects requests exceeding this size before processing.

For pages with file uploads, increase this to match your expected maximum file size:

```apache
SetEnv CONTENT_LENGTH 10
```

## Editing Rate Limits

Open `.htaccess` in your project root and adjust the `SetEnv` values. Changes take effect immediately - no Apache restart needed for `.htaccess` directives (as long as `AllowOverride All` is set in your virtual host configuration).
