---
layout: default
title: turn on basement-desk-light
---
# turn on basement-desk light

Turn on smart plug for my desk light if my motion sensor detects movement

## YAML Template
```
alias: Basement turn off desk light
description: ""
triggers:
  - type: not_occupied
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: binary_sensor
    trigger: device
    for:
      hours: 0
      minutes: 15
      seconds: 0
conditions:
  - condition: sun
    before: sunset
actions:
  - type: turn_off
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: switch
mode: single
```