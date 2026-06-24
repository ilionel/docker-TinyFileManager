# docker-TinyFileManager
The simplest way to use "Tiny File Manager" into a docker environment 

## Status

This repository currently ships only documentation — no `Dockerfile` / `docker-compose.yml`
is included yet. A minimal example and the security checklist below are a starting point.

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

## Minimal example (review before use)

```yaml
# docker-compose.yml — illustrative, not a turnkey image
services:
  tinyfilemanager:
    image: php:8.3-apache            # pin to a digest in production
    ports:
      - "127.0.0.1:8080:80"          # bind to localhost; expose via a proxy with auth
    volumes:
      - ./app:/var/www/html          # place a pinned tinyfilemanager.php (+ config) here
      - ./data:/var/www/html/data    # the directory it manages
    restart: unless-stopped
```

1. Download a pinned `tinyfilemanager.php` release into `./app/`.
2. Edit its config to set strong credentials (and a per-deployment `$auth_users` / salt).
3. `docker compose up -d`, then reach it only through your authenticated proxy.
