---
layout: default
title: actual
---

# actual

[Official Documentation](https://actualbudget.org/docs/)

Actual is a personal finance tool that's built on the principle of local-first software. It allows you to track all of your accounts and budgets in one place, with a focus on privacy and data ownership. It features a powerful envelope budgeting system, multi-device sync, and optional end-to-end encryption.

## Notes

A place for notes about the Actual Budget service.

## Docker Compose Configuration

This is the configuration for the `actual` service.

```yaml
services:
  actual_server:
    container_name: actual
    image: docker.io/actualbudget/actual-server:latest
    ports:
      - '5006:5006'
    volumes:
      - ./actual-data:/data
    healthcheck:
      test: ['CMD-SHELL', 'node src/scripts/health-check.js']
      interval: 60s
      timeout: 10s
      retries: 3
      start_period: 20s
    restart: unless-stopped
``` 