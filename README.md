# Setup

Version 2.0.0

See [CHANGELOG.md](CHANGELOG.md) for what changed in this release compared with the previous version.

For php local development
I used for a reusable Docker stack where each PHP project lives in `projects/<project-folder>`.

## Installation

Please run follow steps below:

```bash
git clone https://github.com/nguyenhongngoc09/devenv.git
cd devenv
bin/setup-project
```

## About

All commands you need are in the `bin` folder

- Database information in 'env/db.env'
- You can create some folders:

```bash
mysql/dbdata/
logs/
projects/<project-folder>
```

`projects/<project-folder>` is where each application source code runs in the shared Docker containers.
The setup flow will ask for the folder name you want to install into.
HTTP is redirected to HTTPS automatically, so use `http://...` only as a fallback.

## Add A New Project

If you add a new project folder manually, follow these steps:

1. Create the folder under `projects/<project-folder>/`.
2. Put the app code inside that folder, including `public/index.php` if the framework uses a `public` document root.
3. Run `bin/refresh-vhosts`.
4. Open `https://<your-base-url>/<project-folder>/` in the browser.
5. If the framework needs extra setup, follow its official install docs inside that project folder.

If you use `bin/setup-project`, it will ask for the project folder and refresh the vhosts for you.

## Optional Services

Redis is kept as an optional service for projects that need cache, queue, or session storage.

To enable Redis quickly:

1. Add a `redis` service in `docker-compose.yml` using `REDIS_VERSION` from `.env`.
2. Keep it on the default Compose network so PHP can reach it by service name `redis`.
3. Start the stack again with `bin/start`.
4. Use `bin/redis <command>` once the service is running.

If a framework needs Redis, follow that framework's own docs for the cache/session/queue config and point it to host `redis`.

Sample app-level settings:

```env
REDIS_HOST=redis
REDIS_PORT=6379
```

## License

[MIT](https://choosealicense.com/licenses/mit/)
