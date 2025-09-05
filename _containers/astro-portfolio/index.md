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
    environment:
      - PUBLIC_GIFTS_PASSWORD=<YOUR_PUB_GIFTS_PW>
      - AUTH_TRUST_HOST=true
      - AUTH_SECRET=<YOUR_AUTH_SECRET>
      - GITHUB_CLIENT_ID=<YOUR_GH_CLIENT_ID>
      - GITHUB_CLIENT_SECRET=<YOUR_GH_CLIENT_SECRET>
      - RESEND_API_KEY=<YOUR_RESEND_API_KEY>
      - EMAIL_ADDRESS=<YOUR_EMAIL>
      - SENTRY_AUTH_TOKEN="<YOUR_SENTRY_AUTH_TOKEN_IN_QUOTES>"
      - ASTRO_TELEMETRY_DISABLED=1
    volumes:
      - ./data:/app/data 

volumes:
  sqlite-data:
```
