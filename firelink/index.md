---
layout: default
title: firelink
---

# firelink

firelink is a custom service from the [rjhoppe/firelink](https://github.com/rjhoppe/firelink) repository. The repository itself serves as the documentation for this service.

## Docker Compose Configuration

This is the configuration for the `firelink` service.

```yaml
services:
  firelink:
    build: .
    ports:
      - "8080:8080"
    environment:
      - POSTGRES_HOST=postgres
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
      - SPOONACULAR_API_KEY=${SPOONACULAR_API_KEY}
    depends_on:
      - postgres

  postgres:
    image: postgres:15
    restart: always
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```
