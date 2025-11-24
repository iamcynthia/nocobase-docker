# NocoBase Docker Compose

Minimal Docker Compose setup for running [NocoBase](https://www.nocobase.com/) with an embedded PostgreSQL database. The runtime data lives under `storage/` so it can persist between restarts while keeping secrets out of Git history.

## Prerequisites
- Docker Engine and Docker Compose v2

## Setup
1) Copy the example environment file and edit values as needed:
```bash
cp .env.example .env
openssl rand -hex 32  # generate a new APP_KEY, then paste it into .env
```
2) Optionally adjust `APP_PORT` or database credentials in `.env`.

## Run
```bash
docker compose up -d    # start NocoBase + Postgres
docker compose logs -f app
```
Stop the stack with `docker compose down`. Data persists under `storage/`.

## Project layout
- `docker-compose.yml` – container definitions; reads values from `.env`
- `storage/` – app data (uploads, logs, DB files, license key); most contents are git-ignored
- `.env.example` – sample configuration for local copies

## Notes
- Keep `.env`, `storage/.license/`, and other runtime data out of GitHub; `.gitignore` is configured accordingly.
- On first run Docker will pull the `nocobase/nocobase:latest-full` and `postgres:16` images. A restart after image updates is typically enough to apply upgrades.
