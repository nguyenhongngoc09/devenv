# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview
This repository is a Docker-based PHP development environment for multiple PHP projects, each mounted under `projects/<project-folder>`.

## Common Commands

### Environment Management
- **Start Environment**: `bin/start`
- **Stop Environment**: `bin/stop`
- **Restart Environment**: `bin/restart`
- **Check Status**: `bin/status`
- **Setup Project**: `bin/setup-project` (Interactive script for git pull, project folder selection, DB import, and SSL setup)
- **Clean Up**: `bin/removeall` (Stop containers and remove volumes)

### Container Interaction
- **Application Shell**: `bin/bash` (Access the `app` container as the current user)
- **Root Shell**: `bin/root` (Access the `app` container as root)
- **Run CLI Command**: `bin/cli <command>` (Run command in the `app` container)
- **Fix Permissions**: `bin/fixperms` (Fixes ownership and permissions in a project folder)

### Development Tools
- **Composer**: `bin/composer <args>`
- **NPM**: `bin/npm <args>`
- **Node**: `bin/node <args>`
- **Xdebug**: `bin/xdebug <on|off>` (Toggle Xdebug on/off)

### Database
- **MySQL CLI**: `bin/mysql`
- **Import Database**: `bin/mysql-import <sql_file>`
- **Export Database**: `bin/mysql-export <output_file>`
- **Redis CLI**: `bin/redis <command>` once the Redis service is enabled in `docker-compose.yml`

## Code Architecture

### Service Structure
- **Nginx (app)**: Serves the application. Version is controlled by `NGINX_VERSION` in `.env`.
- **PHP-FPM (phpfpm)**: Handles PHP processing. Version is controlled by `PHP_VERSION` in `.env`.
- **MySQL (db)**: Database service. Version is controlled by `DB_VERSION` in `.env`. Configured via `env/db.env`.
- **Redis (optional)**: Enable when a project needs cache, queue, or session storage.
### File Locations
- **Source Code**: Application source code resides in `./projects/<project-folder>/` and is mounted to `/var/www/html/projects/<project-folder>` in the containers.
- **Docker Images**: Dockerfiles and configurations are in `images/`.
- **Environment Config**:
    - Main versions: `.env`
    - Database settings: `env/db.env`
- **Logs**: Service logs are stored in `logs/`.
- **Database Data**: MySQL data is persisted in `mysql/dbdata/`.

### Development Workflow
1. Use `bin/setup-project` to initialize a new project from a Git repository.
2. Place source code in `projects/<project-folder>/` (usually done via `bin/setup-project` or manual cloning).
3. After adding a new project folder, run `bin/refresh-vhosts` to regenerate Nginx routes for the new path.
4. Use `bin/composer`, `bin/npm`, `bin/node`, and `bin/xdebug` through the provided wrapper scripts.
5. Install framework dependencies according to the framework's own documentation inside the project folder.
6. Access the web interface via the `BASE_URL` configured during setup (usually requires local `/etc/hosts` entries).
