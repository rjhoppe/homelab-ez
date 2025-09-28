---
layout: default
title: daily turn off bedroom fan
---
Daily task to turn off the smart plug for my bedroom fan

```
alias: Daily turn off bedroom fan
description: ""
triggers:
  - trigger: time
    at: "09:45:00"
    weekday:
      - sun
      - mon
      - tue
      - wed
      - thu
      - fri
      - sat
conditions:
  - condition: device
    device_id: <YOUR_DEVICE_ID>
    domain: device_tracker
    entity_id: <YOUR_ENTITY_ID>
    type: is_home
actions:
  - type: turn_off
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: switch
mode: single
```