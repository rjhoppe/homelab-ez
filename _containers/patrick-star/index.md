---
layout: default
title: patrick-star
---

# patrick-star

Patrick Star is a custom service from the [rjhoppe/patrick-star](https://github.com/rjhoppe/patrick-star) repository. The repository itself serves as the documentation for this service.

## Docker Compose Configuration

This is the configuration for the `patrick-star` service.

```yaml
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
      - WEBHOOK=
```
