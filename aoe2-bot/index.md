---
layout: default
title: aoe2-bot
---

# aoe2-bot

## Notes

A place for notes about the aoe2-bot service.

## Docker Compose Configuration

This is the configuration for the `aoe2-bot` service.

```yaml
services:
  aoe2-bot:
    image: rickjhoppe/aoe2-bot
    container_name: aoe2-bot
    restart: on-failure:3
    ports:
      - "50007:50007"
    environment:
      - TOKEN=TOKEN_VAL
      - BOT_PREFIX=!
      - GUILD_ID=GUILD_ID
      - TEXT_CHAN_ID=TEXT_CHAN_ID
      - LOG_LEVEL=DEBUG
```
