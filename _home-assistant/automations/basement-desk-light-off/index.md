---
layout: default
title: turn off basement-desk-light
---
Turn off smart plug for my desk light if my motion sensor detects movement

```
alias: Basement turn off desk light
description: ""
triggers:
  - type: not_occupied
    device_id: 9214842e98f9cdabdcd32c24931ecb5f
    entity_id: c97ac11bacde16deee4c45ff52c41453
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
    device_id: cf66dfc9e8574dd8c5131b622045b494
    entity_id: cce3b3b6a00a2cc5cb292360aae70b74
    domain: switch
mode: single
```