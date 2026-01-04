---
layout: default
title: changedetection.io
---

# changedetection.io

[Official Documentation](https://changedetection.io/)

changedetection.io is the best and simplest tool for website change detection, web page monitoring, and website change alerts. Perfect for tracking content changes, price drops, restock alerts, and website defacement monitoring.

## Docker Compose Configuration

This is the configuration for the `changedetection.io` service.

```yaml
services:
    changedetection:
      image: ghcr.io/dgtlmoon/changedetection.io
      container_name: changedetection
      hostname: changedetection
      volumes:
        - changedetection-data:/datastore
      # Comment out if you don't need chrome sidecar
      environment:
        - PLAYWRIGHT_DRIVER_URL=ws://playwright-chrome:4567
      ports:
        - 5000:5000
      restart: unless-stopped

    # Comment out whole block if you don't need chrome sidecar
    playwright-chrome:
      image: browserless/chrome:latest
      restart: unless-stopped
      environment:
      - PORT=4567  # Tells the app inside to listen on 4567
      ports:
        - "4567:4567"

volumes:
  changedetection-data:
```
