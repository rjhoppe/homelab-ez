---
layout: default
title: water-leak-alert
---
Alerts for my Aqara water leak sensors that send me a ntfy message and a Home Assistant app notification if the sensors switch from the `dry` to the `moist` state.

It took me a little bit to figure out how to the nfty message format to not look so *ugly* :)

{% raw %}
```
alias: Water leak alert
description: ""
triggers:
  - type: moist
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: binary_sensor
    trigger: device
    for:
      hours: 0
      minutes: 0
      seconds: 5
  - type: moist
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: binary_sensor
    trigger: device
    for:
      hours: 0
      minutes: 0
      seconds: 5
  - type: moist
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: binary_sensor
    trigger: device
    for:
      hours: 0
      minutes: 0
      seconds: 5
  - type: moist
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: binary_sensor
    trigger: device
    for:
      hours: 0
      minutes: 0
      seconds: 5
  - type: moist
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: binary_sensor
    trigger: device
    for:
      hours: 0
      minutes: 0
      seconds: 5
  - type: moist
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: binary_sensor
    trigger: device
    for:
      hours: 0
      minutes: 0
      seconds: 5
conditions: []
actions:
  - device_id: <YOUR_DEVICE_ID>
    domain: mobile_app
    type: notify
    message: >
      The sensor "{{ trigger.to_state.attributes.friendly_name }}" has detected
      a leak.
    title: "WARNING: LEAK DETECTED"
  - choose:
      - conditions:
          - condition: device
            device_id: <YOUR_DEVICE_ID>
            domain: device_tracker
            entity_id: <YOUR_ENTITY_ID>
            type: is_home
        sequence:
          - device_id: <YOUR_DEVICE_ID>
            domain: mobile_app
            type: notify
            message: >-
              The sensor "{{ trigger.to_state.attributes.friendly_name }}" has
              detected a leak.
            title: "WARNING: LEAK DETECTED"
      - conditions:
          - condition: device
            device_id: <YOUR_DEVICE_ID>
            domain: device_tracker
            entity_id: <YOUR_ENTITY_ID>
            type: is_not_home
        sequence:
          - action: notify.ntfy
            metadata: {}
            data:
              message: >-
                The sensor "{{ trigger.to_state.attributes.friendly_name }}" has
                detected a leak.
              title: "WARNING: LEAK DETECTED"
mode: single
```
{% endraw %}