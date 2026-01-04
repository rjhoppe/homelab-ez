---
layout: default
title: lychee
---



## Docker Compose Configuration

This is the configuration for the `lychee` service.

```yaml
services:
  lychee:
    image: lycheeorg/lychee
    container_name: lychee
    restart: unless-stopped
    volumes:
      # Bind mounts for directories you manage on the host
      - ./lychee/uploads:/uploads
      - ./lychee/sym:/sym
      - ./lychee/conf:/conf
    environment:
      - DB_CONNECTION=sqlite
      - APP_URL=<YOUR-APP-URL>
      - TRUSTED_PROXIES=*
      - APP_ENV=production
    ports:
      - "9090:80"
```