---
layout: default
title: ntfy-backup-msg
---
Sends a message to my ntfy instance telling me that my Home Assistant backup to my GDrive has been successful

```
alias: ntfy Backup Notifications
description: Notify when backup starts and completes
triggers:
  - entity_id: sensor.backup_backup_manager_state
    trigger: state
actions:
  - choose:
      - conditions:
          - condition: state
            entity_id: sensor.backup_backup_manager_state
            state: create_backup
        sequence:
          - data:
              title: HA Backup
              message: ☁️ Backup is starting
            action: notify.ntfy
      - conditions:
          - condition: state
            entity_id: sensor.backup_backup_manager_state
            state: idle
        sequence:
          - data:
              title: HA Backup
              message: ✅ Backup completed and uploaded to Google Drive
            action: notify.ntfy
mode: single
```