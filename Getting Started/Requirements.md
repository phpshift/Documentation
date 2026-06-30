# Requirements

Before setting up a PHPShift project, make sure your environment meets the following requirements.

## Operating System

PHPShift is currently designed for **Windows**. The localhost management module directly interacts with:

- `C:/xampp/` - XAMPP installation path
- `C:/Windows/System32/drivers/etc/hosts` - system hosts file
- `C:/xampp/apache/conf/extra/httpd-vhosts.conf` - Apache virtual hosts
- Windows Desktop shortcut creation (`.lnk` files via `win32com`)

Linux and macOS support would require modifications to the localhost management module.

## Required Software

| Software | Purpose |
|----------|---------|
| **XAMPP** | Local Apache + MySQL server (`C:/xampp`) |
| **Python 3.x** | PHPShift CLI runtime |
| **PHP** (via XAMPP) | Project execution |
| **MySQL** (via XAMPP) | Project database |
| **VS Code** | Default editor (PHPShift opens files in `code`) |
| **Git** | Version control (optional but recommended) |

## Python Dependencies

PHPShift uses the following Python packages (installed alongside PHPShift):

- `clight` - PHPShift's CLI rendering and input system
- `aisi` - PHPShift's AI skill interface (prompt assembly + model communication)
- `sqlalchemy` + `pymysql` - Database ORM and MySQL driver
- `win32com` - Windows shortcut creation
- `pyyaml` - YAML config parsing
- `threading`, `subprocess`, `secrets`, `shutil` - Standard library utilities

## AI Model and API Key

You need one of the following:

- **API-key-based model** - Any model supported by the `aisi` module (e.g., Claude, OpenAI-compatible endpoints). You supply the model identifier string and API key.
- **Google Vertex AI** - Requires a service account JSON key file placed in your project directory. The `.env` value `PHPSHIFT_AIKEY` must be the relative path to this file.

You select and configure your AI model during `phpshift new` project setup and can change it later with `More > ChangeAIModel`.

## Database

PHPShift requires a running **MySQL** server (port 3306 by default, via XAMPP). During project creation, you provide:

- Database host (usually `localhost`)
- Database name (PHPShift creates it if it doesn't exist)
- Database user
- Database password

The framework auto-runs `setup.sql` on first start to create system tables (`user_files`, `user_keys`, `api`).
