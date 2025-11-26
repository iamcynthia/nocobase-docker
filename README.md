# NocoBase Docker Compose

Minimal Compose stack for running [NocoBase](https://www.nocobase.com/) with an embedded PostgreSQL database. Data lives under `storage/` so it survives container restarts while staying out of Git history.

## Requirements
- Docker Engine + Docker Compose v2
- Free ports: `13000` (app) and `5432` (Postgres) by default

## Quick start
```bash
cp .env.example .env
openssl rand -hex 32  # generate APP_KEY and paste into .env
docker compose up -d  # start NocoBase + Postgres
docker compose logs -f app
```
Stop with `docker compose down`. Data persists under `storage/`.

## Configuration (.env)
- `APP_KEY` (required): secret used for tokens; use a random 32–64 hex string.
- `APP_PORT` (default 13000): host port mapping to the app container.
- `DB_*`: connection settings; by default point to the bundled Postgres service.
- Optional SSL flags for external DBs: `DB_DIALECT_OPTIONS_SSL_MODE`, `DB_DIALECT_OPTIONS_SSL_REJECT_UNAUTHORIZED`.

Use an existing database by pointing `DB_HOST`/`DB_PORT` to it and, if desired, removing the `postgres` service from `docker-compose.yml`.

## Useful commands
- `docker compose ps` — check container status
- `docker compose logs -f app` — follow app logs
- `docker compose restart app` — quick app restart
- `docker compose up -d app --no-deps` — start/recreate only the app container without touching Postgres
- `docker compose pull && docker compose up -d` — upgrade to the tagged image (`nocobase/nocobase:1.9.12-full`)

## Project layout
- `docker-compose.yml` — container definitions and env wiring
- `storage/` — app data (uploads, logs, DB files, license key)
- `.env.example` — sample configuration to copy for local use

## Notes
- Keep `.env`, `storage/.license/`, and other runtime data out of GitHub (covered by `.gitignore`).
- First start will pull `nocobase/nocobase:1.9.12-full` and `postgres:16`; restarts after pulls are enough to apply updates.
