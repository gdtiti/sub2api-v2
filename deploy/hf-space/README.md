---
title: Sub2API V2
emoji: 🚀
colorFrom: blue
colorTo: green
sdk: docker
app_port: 8080
pinned: false
---

# Sub2API V2

This Space runs Sub2API from a prebuilt image in GitHub Container Registry.

## Required Hugging Face Secrets

- `DATABASE_PASSWORD`
- `REDIS_PASSWORD`
- `ADMIN_PASSWORD`
- `JWT_SECRET`

## Required Hugging Face Variables

- `DATABASE_HOST`
- `DATABASE_PORT`
- `DATABASE_USER`
- `DATABASE_DBNAME`
- `DATABASE_SSLMODE`
- `REDIS_HOST`
- `REDIS_PORT`
- `REDIS_DB`
- `REDIS_ENABLE_TLS`
- `ADMIN_EMAIL`

## Optional Hugging Face Variables

- `RUN_MODE`
- `SERVER_MODE`
- `TIMEZONE`
- `DATA_DIR`

## Notes

- The container enables `AUTO_SETUP=true` and listens on port `8080`.
- If you want config and install markers to survive restarts, attach persistent storage and keep `DATA_DIR=/data`.
- Use PostgreSQL and Redis endpoints that are reachable from Hugging Face Spaces.
