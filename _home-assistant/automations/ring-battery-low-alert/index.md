---
layout: default
title: ring-battery-low-alert
---
A custom battery low alert for a Ring doorbell that sends a message to my Home Assistant Android app and ntfy instance

{% raw %}
```
alias: Ring doorbell camera battery low
description: ""
triggers:
  - type: battery_level
    device_id: <YOUR_DEVICE_ID>
    entity_id: <YOUR_ENTITY_ID>
    domain: sensor
    trigger: device
    below: 20
    for:
      hours: 0
      minutes: 0
      seconds: 5
conditions:
  - condition: template
    value_template: >
      {{ (now() - state_attr('automation.ring_doorbell_camera_battery_low',
      'last_triggered')).total_seconds() > 86400 }}
actions:
  - device_id: c29ddf992fcf23a1d1a5a38e9d9ea9b8
    domain: mobile_app
    type: notify
    message: |
      The Ring doorbell battery needs to be recharged
    title: "WARNING: LOW RING DOORBELL BATTERY"
mode: single
```
{% endraw %}