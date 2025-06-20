---
layout: default
title: mumble
---

# mumble

[Official Documentation](https://www.mumble.info/documentation/)

Mumble is a free, open-source, low-latency, high-quality voice chat application. It's an excellent choice for a homelab because it allows you to self-host your own private, secure voice server for gaming, podcasts, or general communication with friends and family, ensuring your conversations remain private.

## Docker Compose Configuration

This is the configuration for the `Mumble` service.

```yaml
services:
    mumble-server:
        image: mumblevoip/mumble-server:latest
        container_name: mumble
        hostname: murmur
        restart: on-failure:3
        user: root
        volumes:
            - type: bind
              source: ./data/mumble
              target: /data
              read_only: false
            - type: bind
              source: /etc/letsencrypt/archive/mumble.rjhoppe.dev
              target: /certificates
              read_only: false
        environment:
            MUMBLE_CONFIG_SSL_CERT: /certificates/fullchain1.pem
            MUMBLE_CONFIG_SSL_KEY: /certificates/privkey1.pem
            MUMBLE_CONFIG_USERS: 10 # Max amount of users that can connect at the same time
            MUMBLE_CONFIG_USERSPERCHANNEL: 10  # Means unlimited if zero
            MUMBLE_CONFIG_WELCOME_TEXT: ""
            MUMBLE_CONFIG_USERNAME: '[ -=\\w\\[\\]\\{\\}\\(\\)\\@\\|\\.]+'
            MUMBLE_CONFIG_CHANNELNAME: '[ \\-=\\w\\#\\[\\]\\{\\}\\(\\)\\@\\|\\%\\'']+'
            MUMBLE_SUPERUSER_PASSWORD: "Test"
            MUMBLE_REGISTERSERVER: 0
            MUMBLE_VERBOSE: 1
        ports:
            - 64738:64738
            - 64738:64738/udp

```

Mumble env vars to pass in:
`MUMBLE_SUPERUSER_PASSWORD`