---
layout: default
title: umami
---

# umami

[Official Documentation](https://umami.is/docs)

Umami is a simple, fast, privacy-focused alternative to Google Analytics. It provides a self-hosted solution to track website usage statistics without collecting any personal data. In a homelab, it's perfect for monitoring traffic on your personal website or any other web services you host.

## Docker Compose Configuration

This is the configuration for the `umami` service.

```yaml
services:
  umami:
    image: ghcr.io/umami-software/umami:postgresql-latest
    container_name: umami
    ports:
      - 54567:3000
    environment:
      DATABASE_URL: postgresql://admin:<YOUR_DB_PW>@umami-db:5432/umami
      DATABASE_TYPE: postgresql
      APP_SECRET: <YOUR_APP_PW>
    depends_on:
      - umami-db
    restart: unless-stopped

  umami-db:
    image: postgres:15-alpine
    container_name: umami-db
    environment:
      POSTGRES_DB: umami
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: <YOUR_PG_PW>
    volumes:
      - ./umami-db-data:/var/lib/postgresql/data
    restart: unless-stopped
  umami-db-backup:
    container_name: umami-db-backup
    image: tiredofit/db-backup
    volumes:
      - ./backups:/backup
    environment:
      DB_TYPE: postgres
      DB_HOST: umami-db
      DB_NAME: umami
      DB_USER: admin
      DB_PASS: <YOUR_DB_PW>
      DB_BACKUP_INTERVAL: 720
      DB_CLEANUP_TIME: 72000
```
