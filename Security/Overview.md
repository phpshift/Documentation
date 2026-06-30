# Security Overview

PHPShift bakes security into the generated framework from day one. The `.htaccess` file, PHP framework engine, and environment variable system all enforce protections that would take significant effort to implement manually in a typical PHP project.

## Security Layers

### 1. Apache-Level (.htaccess)

Every PHPShift project is initialized with a security-hardened `.htaccess` that:

- **Blocks directory listing** - `Options -Indexes`
- **Denies access to sensitive files** - `.env`, `*.log`, `.system/storage/*`
- **Enforces HTTP security headers** (see [HTAccess Rules](HTAccessRules.md))
- **Restricts HTTP methods** - Only GET, POST, PUT, DELETE are allowed
- **Rate limits** by IP and by session (see [Rate Limiting](RateLimiting.md))
- **Removes server fingerprinting** - `X-Powered-By` header is unset, `ServerSignature Off`

### 2. PHP Framework Engine

The `.system/` PHP engine:

- Routes all requests through a single entry point (`app.php`)
- Validates API authentication tokens before dispatching
- Enforces access group checks before serving any page
- Parameterizes all database queries (no raw SQL string interpolation)
- Validates request size against `CONTENT_LENGTH` limit

### 3. Generated Code Practices

The AI generation prompts enforce:

- Input validation in every PHP method before processing
- No `App::redirect()` in PHP (redirects handled by JS to avoid open redirect vulnerabilities)
- Environment variables via `App::env()` - never hardcoded credentials
- Parameterized queries in all `DB::*` calls
- `private` visibility for all non-callable PHP helper methods

## What PHPShift Does Not Handle

- **HTTPS/TLS** - Configure SSL certificates at your hosting/server level, not in PHPShift
- **Firewall rules** - Network-level protection is the server's responsibility
- **File upload scanning** - The framework limits file size but does not scan content; add AV scanning via Composer plugins if needed
- **Production secrets management** - Use a secrets vault on your production server; `.env` is for local development config
