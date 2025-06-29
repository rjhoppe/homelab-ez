---
layout: default
title: astro-portfolio
---

# astro-portfolio

Astro is a modern web framework for building fast, content-driven websites. It's known for its "islands" architecture, which helps to reduce the amount of JavaScript shipped to the browser, resulting in faster load times.

For more information, you can refer to the [official documentation](https://docs.astro.build/).

## Docker Compose Configuration

This is the configuration for the `astro-portfolio` service.

```yaml
services:
  app:
    image: rickjhoppe/astro-portfolio:latest
    restart: on-failure:3
    container_name: astro-portfolio
    ports:
      - "127.0.0.1:4321:4321"
    volumes:
      - sqlite-data:/data

volumes:
  sqlite-data:
```
