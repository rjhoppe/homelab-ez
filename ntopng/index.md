---
layout: default
title: ntopng
---

# ntopng

[Official Documentation](https://www.ntop.org/guides/ntopng/)

ntopng is a web-based network traffic monitoring application that provides a high-level overview of network traffic. In a homelab, it's incredibly useful for visualizing network usage, identifying bandwidth hogs, and troubleshooting network issues.

## Notes

A place for notes about the ntopng service.

## Docker Compose Configuration

This is the configuration for the `ntopng` service.

```yaml
services:
  ntopng:
    container_name: ntopng
    image: vimagick/ntopng
    command:
      - --community
      - -d
      - /var/lib/ntopng
      - -i
      - eth0
      - -r
      - 127.0.0.1:6379@0
      - -w
      - 0.0.0.0:9888
    volumes:
      - ntopng_data:/var/lib/ntopng
    network_mode: host
    restart: unless-stopped

  redis:
    image: redis:alpine
    network_mode: host
    volumes:
      - redis_data:/data
    restart: unless-stopped

volumes:
  ntopng_data:
  redis_data:
```
