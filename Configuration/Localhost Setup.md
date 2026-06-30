# Localhost Setup

PHPShift automates local development server configuration using XAMPP on Windows. When you run `phpshift start`, it sets up everything needed to access your project via a custom `.local` domain - no manual Apache or hosts file editing required.

## What PHPShift Configures

### 1. Windows Hosts File

PHPShift adds an entry to `C:/Windows/System32/drivers/etc/hosts`:

```
# phpshift_myapp-hosts
127.0.0.1 myapp.local
# phpshift_myapp-host
```

The comment markers (`# phpshift_myapp-hosts` and `# phpshift_myapp-host`) are used to identify and remove this entry cleanly when the project is stopped.

### 2. Apache Virtual Host

PHPShift adds a virtual host block to `C:/xampp/apache/conf/extra/httpd-vhosts.conf`:

```apache
# phpshift_myapp-vhost
<VirtualHost *:80>
    DocumentRoot "C:/projects/myapp"
    ServerName myapp.local
    <Directory "C:/projects/myapp">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

Again, the comment marker (`# phpshift_myapp-vhost`) allows PHPShift to locate and remove this block cleanly on stop.

### 3. XAMPP Start

If Apache and MySQL are not already running, PHPShift starts them by executing:

```
C:/xampp/xampp_start.exe
```

## Checking Localhost Status

PHPShift checks whether a project's localhost is already configured by:

1. Verifying that `httpd.pid` and `mysql.pid` exist (XAMPP running)
2. Finding the project's virtual host marker in `httpd-vhosts.conf`
3. Finding the project's host marker in `hosts`

If all three conditions are met, the start step is skipped (fast startup for already-configured projects).

## Stopping Localhost

When you exit PHPShift (`phpshift stop` or menu exit), it:

1. Removes the virtual host block from `httpd-vhosts.conf`
2. Removes the hosts entry from the Windows hosts file
3. Optionally restarts Apache to apply the changes

## Multiple Projects

Each PHPShift project uses a unique marker tag (`phpshift_{hint}`) based on the local domain name. Multiple projects can coexist with separate virtual hosts. Stopping one project does not affect others.

## Port Configuration

PHPShift uses port 80 by default (standard HTTP). If another service occupies port 80, you'll need to adjust XAMPP's Apache port configuration manually in `C:/xampp/apache/conf/httpd.conf` before starting PHPShift.

## Troubleshooting

**XAMPP not found** - Ensure XAMPP is installed at exactly `C:/xampp`. Custom paths are not currently supported without modifying the `localhost.py` module.

**Permission denied on hosts file** - PHPShift requires admin privileges to write to the Windows hosts file. Run your terminal as Administrator when starting PHPShift.

**Virtual host not loading** - Ensure Apache's `httpd-vhosts.conf` include is uncommented in `httpd.conf`. Look for `Include conf/extra/httpd-vhosts.conf` and make sure it's not commented out.
