---
layout: default
title: ha configuration.yaml
---

## Home Assistant configuration.yaml file

This is the main config file for your Home Assistant OS instance. You will need an add-on like the popular File Editor add-on to edit this file from the Home Assistant GUI.

```
# Loads default set of integrations. Do not remove.
default_config:

# Load frontend themes from the themes folder
frontend:
  themes: !include_dir_merge_named themes
  
notify:
  - name: ntfy
    platform: rest
    resource: "<YOUR_NTFY_URL_IN_QUOTES>"
    method: POST_JSON
    message_param_name: ""

automation: !include automations.yaml
script: !include scripts.yaml
scene: !include scenes.yaml
```