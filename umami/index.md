---
layout: default
title: umami
---

# umami

[Official Documentation](https://umami.is/docs)

Umami is a simple, fast, privacy-focused alternative to Google Analytics. It provides a self-hosted solution to track website usage statistics without collecting any personal data. In a homelab, it's perfect for monitoring traffic on your personal website or any other web services you host.

## Notes

A place for notes about the umami service.

## Docker Compose Configuration

This is the configuration for the `umami` service.

```yaml
services:
  umami:
    image: ghcr.io/umami-software/umami:postgresql-latest
    container_name: umami
    ports:
      - 3000:3000
    environment:
      DATABASE_URL: postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@umami-db:5432/${POSTGRES_DB}
      DATABASE_TYPE: postgresql
      APP_SECRET: ${APP_SECRET}
    depends_on:
      umami-db:
        condition: service_healthy
    restart: unless-stopped
    healthcheck:
      test:
        - CMD-SHELL
        - curl http://localhost:3000/api/heartbeat
      interval: 5s
      timeout: 5s
      retries: 5
  umami-db:
    image: postgres:15-alpine
    container_name: umami-db
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - ./umami-db-data:/var/lib/postgresql/data
    restart: unless-stopped
    healthcheck:
      test:
        - CMD-SHELL
        - pg_isready -U $${POSTGRES_USER} -d $${POSTGRES_DB}
      interval: 5s
      timeout: 5s
      retries: 5
  umami-db-backup:
    container_name: umami-db-backup
    image: tiredofit/db-backup
    volumes:
      - ./backups:/backup
    environment:
      DB_TYPE: postgres
      DB_HOST: umami-db
      DB_NAME: ${POSTGRES_DB}
      DB_USER: ${POSTGRES_USER}
      DB_PASS: ${POSTGRES_PASSWORD}
      DB_BACKUP_INTERVAL: 720
      DB_CLEANUP_TIME: 72000
```
