# Installation

PHPShift is distributed as a Python package via the `clight` ecosystem. The installation process sets up the PHPShift CLI command, its internal `.system` frame, and all required Python dependencies.

## Install PHPShift

Install PHPShift using pip:

```bash
pip install phpshift
```

This makes the `phpshift` command available globally in your terminal.

## Verify Installation

```bash
phpshift --version
```

You should see the current PHPShift version number.

## XAMPP Setup

Ensure XAMPP is installed at `C:/xampp` with both Apache and MySQL modules enabled. PHPShift looks for:

```
C:/xampp/xampp_start.exe
C:/xampp/apache/logs/httpd.pid
C:/xampp/mysql/data/mysql.pid
C:/xampp/apache/conf/extra/httpd-vhosts.conf
```

If XAMPP is already running when you start a PHPShift project, PHPShift will detect it and skip the start step.

## VS Code Setup (Recommended)

PHPShift automatically opens files in VS Code using the `code` command. Make sure VS Code's CLI is in your system PATH:

1. Open VS Code
2. Press `Ctrl+Shift+P` → type `Shell Command: Install 'code' command in PATH`
3. Confirm installation

PHPShift will open your `.md` briefing file, `.env` config, and SQL migration files in VS Code automatically during the workflow.

## First Run Check

After installation, navigate to an empty folder where you want your first project and run:

```bash
phpshift new
```

PHPShift will begin the interactive project creation process. See **Getting Started → First Project** for a full walkthrough.

## Updating PHPShift

To update PHPShift to the latest version:

```bash
phpshift update
```

Or via pip:

```bash
pip install --upgrade phpshift
```

The `PHPShift > Update` skill inside the development session also handles updates from within the tool.
