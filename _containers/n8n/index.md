---
layout: default
title: n8n
---

# n8n

[Official Documentation](https://docs.n8n.io/)

n8n is a free and source-available workflow automation tool. It enables you to connect various web services and APIs to automate your tasks. It is a no-code/low-code platform that allows for easy creation of complex workflows.

## Docker Compose Configuration

This is the configuration for the `n8n` service.

```yaml
services:
  n8n:
    image: docker.n8n.io/n8nio/n8n # Using the official image source
    container_name: n8n            # Assign a clear container name
    restart: unless-stopped        # Use unless-stopped for better control
    
    ports:
      # Exposes port 5678 on your mini PC's network interface
      # Change 5678:5678 to 8080:5678 if port 5678 is blocked
      - "5678:5678"
      
    # ⬇️ Security/Permission Fix: Set the n8n user ID to 1000 
    # This is the default user ID inside the n8n container, ensuring it can write to the volume.
    environment:
      # CRITICAL: SET THESE TO YOUR MINI PC'S STATIC IP
      - N8N_HOST=<YOUR-IP> # ⚠️ REPLACE with your Mini PC's actual IP
      - N8N_PROTOCOL=http
      - WEBHOOK_URL=http://<YOUR-IP>/ # ⚠️ REPLACE with your Mini PC's actual IP

      # Optional: Add security/configuration environment variables
      - N8N_USER_ID=1000           # Ensures file ownership consistency
      - N8N_GROUP_ID=1000          # Ensures file ownership consistency
      - NODE_ENV=production
      
      # ⚠️ IMPORTANT: Set up Basic Authentication for security
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=<YOUR-USER>
      - N8N_BASIC_AUTH_PASSWORD=<YOUR-USER-PW>
      - N8N_SECURE_COOKIE=false
      
      # Timezone settings (Optional)
      - GENERIC_TIMEZONE=America/New_York # ⚠️ Change to your timezone
      - TZ=America/New_York
      
    volumes:
      # Mounts a local directory (n8n_data) to the container's data path
      - n8n_data:/home/node/.n8n
      # If you still want to mount local files:
      # - ./local-files:/files 
      
volumes:
  n8n_data: # Define the named volume for persistent storage
```
