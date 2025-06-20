---
layout: default
title: pmp-summary
---

# pmp-summary

## Notes

A place for notes about the pmp-summary service.

## Docker Compose Configuration

This is the configuration for the `pmp-summary` service.

```yaml
services:
  pmp-summary:
    image: rickjhoppe/pmp-summary:latest
    container_name: pmp-summary
    environment:
      - MISTRAL_API_KEY=
      - WASTEBIN_URL=
      - NTFY_URL=
      - WEBHOOK_URL=
```
