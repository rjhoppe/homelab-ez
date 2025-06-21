---
layout: default
title: watchtower
---

# watchtower

Watchtower is a service that monitors running Docker containers and watches for changes to the images that those containers were originally started from. If watchtower detects that an image has changed, it will automatically restart the container using the new image.

For more information, you can refer to the [official documentation](https://containrrr.dev/watchtower/).

## Docker Compose Configuration

This is the configuration for the `watchtower` service.

```yaml
services:
  watchtower:
    container_name: watchtower
    image: containrrr/watchtower
    environment:
      - TZ=America/New_York
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_INCLUDE_STOPPED=true
      - WATCHTOWER_POLL_INTERVAL=86400
      - WATCHTOWER_REVIVE_STOPPED=true
      - WATCHTOWER_TIMEOUT=60s
      - WATCHTOWER_LOG_FORMAT=pretty
      - WATCHTOWER_NOTIFICATIONS=shoutrrr
      - WATCHTOWER_NOTIFICATION_URL=ntfy://ntfy.rjhoppe.dev/[your-topic-here]?scheme=https
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
```
