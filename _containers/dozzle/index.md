---
layout: default
title: dozzle
---

# dozzle

[Official Documentation](https://dozzle.dev/)

Dozzle is a lightweight, web-based log viewer for Docker containers. It provides a simple, real-time interface for monitoring container logs without storing any log data itself. It's incredibly useful for live monitoring and debugging of your containerized applications.

## Docker Compose Configuration

This is the configuration for the `dozzle` service.

```yaml
services:
  app:
    container_name: dozzle
    image: amir20/dozzle:v8.11.7
    restart: on-failure:3
    environment:
      - TZ=America/New_York
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - "127.0.0.1:8888:8080"
```
