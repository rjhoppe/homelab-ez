---
layout: default
title: uptime kuma
---

# uptime kuma

[Official Documentation](https://github.com/louislam/uptime-kuma/wiki)

Uptime Kuma is a self-hosted monitoring tool. It's an easy-to-use and feature-rich service that allows you to monitor the uptime of your HTTP(s) services, TCP ports, DNS records, and more. For a homelab, it is an essential tool for keeping an eye on all your services and getting notified when something goes down.

## Docker Compose Configuration

This is the configuration for the `uptime kuma` service.

```yaml
services:
  uptime-kuma:
    image: louislam/uptime-kuma:1
    container_name: uptime-kuma
    restart: on-failure:3
    ports:
      - "3010:3001"
    volumes:
      - uptime-kuma:/app/data
      - /var/run/docker.sock:/var/run/docker.sock

volumes:
  uptime-kuma:
```
