---
layout: default
title: glances
---

# glances

[Official Documentation](https://glances.readthedocs.io/en/latest/)

Glances is a cross-platform system monitoring tool that presents a large amount of information in a minimal amount of space. It can be accessed via a terminal-based interface or a web UI. It's highly valuable in a homelab for getting a quick, comprehensive overview of your server's resource usage (CPU, memory, disk, network, etc.) in real-time.

## Docker Compose Configuration

This is the configuration for the `glances` service.

```yaml
services:
  glances:
    container_name: glances
    image: nicolargo/glances:latest
    mem_limit: 4g
    cpu_shares: 768
    security_opt:
      - no-new-privileges:true
    pid: host
    restart: on-failure:3
    ports:
      - "127.0.0.1:61208:61208"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    environment:
      TZ: America/New_York
      GLANCES_OPT: -w
```
