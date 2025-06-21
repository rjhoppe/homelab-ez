---
layout: default
title: watch-your-lan
---

# watch-your-lan

WatchYourLAN is a lightweight, self-hosted network IP scanner with a web GUI. It can be used to notify you about new hosts on your network and to monitor the online/offline history of existing hosts.

For more information, you can refer to the [official documentation](httpss://github.com/aceberg/WatchYourLAN).

## Docker Compose Configuration

This is the configuration for the `watch-your-lan`.

```yaml
services:
  wyl:
    container_name: watchyourlan
    image: aceberg/watchyourlan
    network_mode: "host"      
    restart: unless-stopped
    volumes:
    - ~/.dockerdata/wyl:/data/WatchYourLAN
    environment:
      TZ: America/New_York           # required: needs your TZ for correct time
      IFACES: "eth0"
      SHOUTRRR_URL: {NTFY_HERE} # optional, set url to notify
      THEME: "darly"                    # optional
      COLOR: "dark"                     # optional
```