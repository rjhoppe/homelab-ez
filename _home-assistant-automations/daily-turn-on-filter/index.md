---
layout: default
title: daily turn on air filter
---
# daily turn on air filter

Daily task to turn on the smart plug for my living room air filter

## YAML Template
```
alias: Daily turn on air filter
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
  - type: turn_on
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: switch
mode: single
```