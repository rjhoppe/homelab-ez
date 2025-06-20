---
layout: default
title: wastebin
---

# wastebin

Wastebin is a minimalist, self-hosted pastebin service. It's written in Rust and uses a sqlite3 backend, making it lightweight and fast. It supports syntax highlighting, file uploads, and has a clean, simple interface.

For more information, you can refer to the [official documentation](https://github.com/matze/wastebin).

## Docker Compose Configuration

This is the configuration for the `wastebin`.

```yaml
services:
  wastebin:
    container_name: wastebin
    restart: always
    ports:
      - "7088:8088"
    volumes:
      - './data:/data'
    image: 'quxfoo/wastebin:latest'
```
