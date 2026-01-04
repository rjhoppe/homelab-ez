---
layout: default
title: lychee
---

Lychee is a stunning and user-friendly photo management system that you can run on your own server. It's a free, open-source tool designed to help you upload, manage, and share your photos seamlessly, much like a native application. With Lychee, you maintain full control over your images, ensuring they are stored securely and accessible from anywhere.

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