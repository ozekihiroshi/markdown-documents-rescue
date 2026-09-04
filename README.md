# Markdown Documents Rescue

Local Docker environments for developing and release-testing Ozeki Markdown
Documents.

## Safety boundary

Two independent WordPress installations are provided:

- Development: <http://localhost:8087>
- ZIP test: <http://localhost:8088>

The development service bind-mounts the plugin source **read-only**. Changes in
the local repository are visible immediately, but WordPress cannot delete or
overwrite the source tree.

The ZIP-test service has no source bind mount. Install the generated release
ZIP through WordPress administration and use this environment for deletion,
uninstall, and clean-install tests.

Never use the development site to test plugin deletion. Use the ZIP-test site.

## Start

Optional local configuration:

```bash
cp .env.example .env
```

Start the development environment:

```bash
docker compose up -d db-dev wordpress-dev
```

Start the isolated ZIP-test environment:

```bash
docker compose up -d db-ziptest wordpress-ziptest
```

Inspect mounts before any deletion test:

```bash
docker inspect markdown-documents-rescue-wordpress-ziptest-1 \
  --format '{{range .Mounts}}{{println .Type .Source "->" .Destination}}{{end}}'
```

The ZIP-test container must not show a bind mount at
`/var/www/html/wp-content/plugins/ozeki-markdown-documents`.

## WP-CLI

Development:

```bash
docker compose run --rm wp-cli-dev wp --info
```

ZIP test:

```bash
docker compose run --rm wp-cli-ziptest wp --info
```

## Data

Development and ZIP-test databases and WordPress files use separate named
volumes. No cleanup or volume-removal command is provided by this repository.
