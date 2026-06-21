# Changelog

## 2.0.0

Released as the new multi-project PHP dev stack.

### Changed

- Moved from a single source tree to multiple project folders under `projects/<project-folder>`.
- Nginx now generates per-project vhosts automatically from the folders inside `projects/`.
- Each project gets its own logs under `logs/<project-folder>/access.log` and `logs/<project-folder>/error.log`.
- HTTP now redirects to HTTPS by default.
- The stack is now centered around shared containers for many PHP frameworks instead of framework-specific helpers.
- Updated container images to newer versions:
  - Nginx `1.23`
  - PHP `8.1.7`
- Updated setup scripts to ask for the project folder during installation.
- Kept Xdebug in the stack.
- Kept Redis as an optional service for future use.

### Removed

- Legacy copy helpers for syncing files between host and container.
- Framework-specific install helpers such as `bin/laravel`, `bin/symfony`, and `bin/wordpress`.
- The old external source layout outside `projects/`.

### Notes

- To add a new app, create a folder inside `projects/`, place the framework code there, then run `bin/refresh-vhosts`.
- If the app uses a `public` document root, make sure `public/index.php` exists inside that project folder.
- Follow the framework's own installation guide for any app-specific dependencies or config.
