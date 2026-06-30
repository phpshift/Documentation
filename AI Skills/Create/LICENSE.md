# Create > LICENSE

The `Create > LICENSE` skill installs a license file in your project root from PHPShift's built-in license library.

## Available Licenses

- MIT License
- Apache Software License 2.0
- GNU General Public License v3 (GPLv3)
- GNU Affero General Public License v3 (AGPLv3)
- GNU Lesser General Public License v3 (LGPLv3)
- Mozilla Public License 2.0 (MPL 2.0)
- BSD License
- ISC License (ISCL)

## What It Does

PHPShift presents an interactive selection menu. After you choose a license:

1. The license template is read from PHPShift's internal `licenses/` directory
2. `{{year}}` is replaced with the current year
3. `{{author}}` is replaced with `PROJECT_AUTHOR` from your `.env`
4. The file is written to `{project}/LICENSE`

## Example

Selecting MIT License with `PROJECT_AUTHOR="Jane Doe"` and current year 2025 produces:

```
MIT License

Copyright (c) 2025 Jane Doe

Permission is hereby granted, free of charge, to any person obtaining a copy...
```

## Visibility in Menu

This skill only appears in the menu when no `LICENSE` file exists. After creation, the menu switches to `Edit > LICENSE` and `Delete > LICENSE`.
