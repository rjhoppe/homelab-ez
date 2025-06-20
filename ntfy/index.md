---
layout: default
title: ntfy
---

# ntfy

[Official Documentation](https://docs.ntfy.sh/)

ntfy (pronounced "notify") is a simple, HTTP-based publish-subscribe notification service. It allows you to send push notifications to your phone or desktop from any script or application. For a homelab, it's perfect for getting alerts from your services, such as backup completion, failed cron jobs, or security alerts.

## Notes

A place for notes about the ntfy service.

## Usage Example

Shamelessly lifted from here: https://docs.ntfy.sh/examples/#__tabbed_1_1

Modify this first:
```
# at the end of the file
session optional pam_exec.so /usr/bin/ntfy-ssh-login.sh
```

script placed at /usr/bin/ntfy-ssh-login.sh
```
#!/bin/bash
if [ "${PAM_TYPE}" = "open_session" ]; then
  curl \
    -H prio:high \
    -H tags:warning \
    -d "SSH login: ${PAM_USER} from ${PAM_RHOST}" \
    http://127.0.0.1/port/subscription
fi
```

Give permissions
```
chmod +x /usr/bin/ntfy-ssh-login.sh
```

## Docker Compose Configuration

This is the configuration for the `Ntfy` service.

```yaml
services:
  ntfy:
    image: binwiederhier/ntfy
    container_name: ntfy
    command:
      - serve
    environment:
      - TZ=America/New_York   # optional: set desired timezone
    user: 1003:1003 # replace with the user/group or uid/gid
    volumes:
      - ./cache:/var/cache/ntfy
      - ./config:/etc/ntfy
      - ./db:/var/lib/ntfy/
    ports:
      - 8000:80 # exposed on port 8000 (you can change it)
    restart: unless-stopped
```
