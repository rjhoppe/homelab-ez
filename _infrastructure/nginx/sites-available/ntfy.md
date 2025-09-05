---
layout: default
title: sites-available ntfy
---

## NGINX Sites Available NTFY 

This is the nginx sites-available configuration for the `ntfy` service.

```
server {
    server_name <YOUR_NFTY_SERVER>;

    location / {
        proxy_pass http://localhost:8000; # Replace with your>
        proxy_set_header Host $host;
        proxy_set_header X-REAL-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forward>
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";

        # To allow GET from anywhere, but disable POST
        limit_except GET {
            allow <IP_ADDRESS_ONE>;
            allow 127.0.0.1;
            allow <IP_ADDRESS_TWO>;
            deny all;
        }
    }

    certbot config here...
}
```