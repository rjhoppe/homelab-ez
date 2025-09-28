---
layout: default
title: zigbee2mqtt lxc
---

To access the terminal for this particular LXC using your main Proxmox root credentials:
```
pct enter <LXC-ID>
```

To get access to your zigbee2mqtt configuration.yaml file (once in the terminal for your LXC):
```
sudo nano /opt/zigbee2mqtt/data/configuration.yaml
```

This is what my configuration.yaml file looks like:
```
homeassistant:
  enabled: true
frontend:
  enabled: true
  port: 9442
mqtt:
  base_topic: zigbee2mqtt
  server: mqtt://<YOUR_IP_HERE>:1883
  user: mqtt_admin
  password: <YOUR_PW_HERE>
  keepalive: 60
  reject_unauthorized: true
  version: 4
serial:
  port: >-
    /dev/serial/by-id/usb-ITead_Sonoff_Zigbee_3.0_USB_Dongle_Plus_eadad137dd49ef119a5acd8cff00cc63-if00-port0
  adapter: zstack
advanced:
  pan_id: 30820
  ext_pan_id:
    - 112
    - 132
    - 141
    - 185
    - 69
    - 26
    - 165
    - 184
  network_key:
    - 216
    - 199
    - 140
    - 25
    - 227
    - 191
    - 93
    - 89
    - 3
    - 181
    - 58
    - 107
    - 115
    - 10
    - 128
    - 147
  log_level: error
version: 4
devices:
  <This section will be populated as you pair devices>
```

#### NOTE

You may need to manually hardcode a 2.4 GHz band channel range for either your WiFi or your Zigbee controller. If you router is set to dynamically change channels to improve it's connection, it may broadcast on the same channel as your Zigbee controller. This can cause your devices to go offline. There are two ways of resolving this:

1. Hardcode a channel value in your router config for 2.4 GHz WiFi band that is far away from the value that is used by your controller.

2. Hardcode a channel value in your Zigbee2MQTT config that is far away from the values that are typically used by your WiFi's 2.4 GHz band.

I first tried to manually set my Zigbee2MQTT config, but I didn't have much luck with improving the connection stability of my smart devices. Once I set my router to use `channel: 1` for WiFi that resolved my issues. I just present both options in case the Zigbee2MQTT config option is more desirable / better for you.

I did this after all of my Third Reality smart plugs were bricked after a OTP firmware update failed to complete due to a WiFi / controller channel disruption. This might not be necessary for all network configurations, but I found that it helped resolve my issues.


To stop the service (if you need to make config changes):
```
sudo systemctl stop zigbee2mqtt
```

To check status of the service:
```
sudo systemctl status zigbee2mqtt
```

To start the service again:
```
sudo systemctl start zigbee2mqtt
```