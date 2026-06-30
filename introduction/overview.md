# Overview

PHPShift is the world's first AI-powered full-stack PHP framework, launched on **December 19, 2024**. It eliminates the traditional development grind by letting you describe what you want in plain language - and having AI generate production-ready PHP pages, REST APIs, cron jobs, translations, SEO markup and database schemas for you.

At its core, PHPShift is a **CLI-driven development environment** built on Python, wrapping a structured PHP project frame. You interact with it through a terminal menu; it handles localhost orchestration (XAMPP), database management (MySQL via SQLAlchemy), Git version control and AI code generation - all from one unified interface.

## What PHPShift Is

PHPShift is not a library you import. It is a **project generator and manager**: you run it alongside your project directory and it scaffolds, modifies and maintains your PHP web application through AI conversations and structured skill prompts.

Every generation action in PHPShift is called a **Skill**. Skills are categorized by intent: Create, Edit, Delete, Read, Render and More. Each Skill carries a carefully engineered AI prompt that understands your project's existing codebase, database schema and PHP method surface before producing any code.

## Project Structure at a Glance

A PHPShift-managed project contains these top-level elements:

- **Pages/** - Full-stack pages (HTML + CSS + JS + PHP class per page)
- **Apis/** - Standalone PHP REST API files
- **Crons/** - PHP cron job scripts managed by PHPShift's cron thread
- **Space/** - SEO metadata, language translation JSON files and logs
- **.system/** - Internal PHP framework engine (router, ORM, access control, asset pipeline)
- **.assets/** - Public static assets (JS, CSS, images, media)
- **.env** - Project environment configuration
- **.access** - PHP access-group authorization class
- **.md** - Your project plan document (AI-readable briefing file)
- **README.md** - Auto-generated or AI-assisted project readme

## Who Builds PHPShift Projects

PHPShift targets developers and technical builders who want to move fast on full-stack PHP projects without writing every line manually. It is ideal for:

- Solo developers shipping MVPs quickly
- Teams standardizing on a consistent PHP project structure
- Developers who prefer describing features in English over writing boilerplate
- Anyone comfortable in a terminal who wants AI as a coding pair-programmer inside their toolchain
