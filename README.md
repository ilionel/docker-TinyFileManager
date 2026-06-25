# docker-TinyFileManager

[![CI](https://github.com/ilionel/docker-TinyFileManager/actions/workflows/ci.yml/badge.svg)](https://github.com/ilionel/docker-TinyFileManager/actions/workflows/ci.yml)

The simplest way to run [TinyFileManager](https://github.com/prasathmani/tinyfilemanager) in a Docker environment.

## Status

This repo ships a hardened **`docker-compose.yml`** starting point and the security
checklist below. TinyFileManager itself (`tinyfilemanager.php`) is **not** bundled — you
provide a pinned copy (see Quick start).

## ⚠️ Security

[TinyFileManager](https://github.com/prasathmani/tinyfilemanager) is a powerful single-file
PHP file manager. Misconfigured, it exposes your filesystem. Before exposing it:

- **Change the default credentials immediately.** Stock builds ship with
  `admin / admin@123` and `user / 12345` — anyone who finds the page can log in otherwise.
- **Pin a recent release.** Older versions have known authentication-bypass / upload CVEs;
  always run an up-to-date `tinyfilemanager.php`.
- **Never expose it directly on the Internet.** Put it behind a VPN or a reverse proxy with
  its own authentication (e.g. basic-auth / SSO), and serve it over HTTPS.
- **Scope the mounted directory.** Only mount the folder it must manage — never `/` or your
  whole home directory.
- **Disable it if unused.** It is an interactive shell-into-your-files; treat it accordingly.

## Quick start

A hardened starting point is in [`docker-compose.yml`](docker-compose.yml) (bound to
`127.0.0.1`, scoped volumes, `restart: unless-stopped`). Then:

1. Download a pinned `tinyfilemanager.php` release into `./app/`.
2. Edit its config to set strong credentials (and a per-deployment `$auth_users` / salt).
3. `docker compose up -d`, then reach it only through your authenticated proxy.
