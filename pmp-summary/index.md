---
layout: default
title: pmp-summary
---

# pmp-summary

The pmp-summary is a custom service from the [rjhoppe/pmp-summary](https://github.com/rjhoppe/pmp-summary) repository. The repository itself serves as the documentation for this service.

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
