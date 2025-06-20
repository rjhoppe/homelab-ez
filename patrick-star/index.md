---
layout: default
title: Patrick Star
---

# Patrick Star

## Notes

A place for notes about the Patrick Star service.

## Docker Compose Configuration

This is the configuration for the `Patrick Star` service as found in the main `docker-compose.yaml`.

```yaml
# patrick-star
services:
  patrick-star:
    container_name: patrick-star
    image: rickjhoppe/patrick-star:latest
    environment:
      - URL=
      - STORE_PAGE1=
      - STORE_PAGE2=
      - STORE_PAGE3=
      - STORE_PAGE4=
      - STORE_PAGE5=
      - NTFY_URL=
      - WEBHOOK_URL=
```
