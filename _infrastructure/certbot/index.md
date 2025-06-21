---
layout: default
title: certbot
---

# certbot

## What is certbot?

Certbot is a free, open-source software tool for automatically using Let's Encrypt certificates on manually-administered websites to enable HTTPS. It's developed by the Electronic Frontier Foundation (EFF) and is the most commonly used client for Let's Encrypt.

## Why Use certbot in a homelab?

In a homelab environment, you are often running multiple web services, dashboards, and applications. Using certbot to secure these services provides several key benefits:

*   **Encryption:** It enables HTTPS (SSL/TLS) for your services, encrypting the traffic between your browser and the server. This is crucial for protecting sensitive information like login credentials, even on a local network.
*   **Browser Trust:** Modern web browsers will show a "Not Secure" warning for any site served over plain HTTP. By using valid certificates from Let's Encrypt, your internal services will get the green padlock, eliminating security warnings and making them feel more professional.
*   **It's Free:** Let's Encrypt provides SSL/TLS certificates completely free of charge.
*   **Automation:** The best part of Certbot is its ability to automate the entire process of obtaining and, more importantly, renewing certificates. You can set it up once and forget about it, which is ideal for a low-maintenance homelab.

Below you will find a collection of useful commands and configurations for managing certbot and Nginx.

Check active ssl certs
```
sudo ls -l /etc/letsencrypt/live/ 
```

To add a new subdomain run these cmds
```
sudo certbot certonly --nginx -d sub.domain.dev
sudo nano /etc/nginx/sites-available/sub.domain.dev
sudo ln -s /etc/nginx/sites-available/sub.domain.dev /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
sudo certbot install --cert-name sub.domain.dev
```

NOTE: Must add a new DNS A record to your DNS provider of the subdomain first or else certbot will fail the validation challenge

Bare bones Nginx config (before certbot install)

```
server {
    listen 80;
    listen [::]:80;

    server_name <subdomain.domain.com>;
    allow <your IP address>;
    deny all;

    location / {
        proxy_pass http://localhost:0000; # Replace with your application's address
        proxy_set_header Host $host;
        proxy_set_header X-REAL-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Cronjob and script to auto-renew certs:

certbot-renew.sh
```
#!/bin/bash

# Run certbot and capture output
RENEW_OUTPUT=$(certbot renew 2>&1)

# Path to your Nginx configuration file (optional check)
NGINX_CONF="/etc/nginx/nginx.conf"

# Check if any certificate was renewed
if echo "$RENEW_OUTPUT" | grep -q "Congratulations! Your certificate"; then
    echo "✅ Certificate was renewed."

    # Reload or start Nginx
    if systemctl is-active --quiet nginx; then
        echo "🔄 Nginx is running, reloading..."
        systemctl reload nginx
    else
        echo "▶️ Nginx is not running, starting..."
        systemctl start nginx
    fi

    # Send ntfy notification
    curl -H "Title: SSL Certificate Renewed" \
         -H "Tags: lock,green" \
         -d "One or more SSL certificates were renewed and Nginx has been reloaded." \
         http://127.0.0.1:8000/[YOUR-TOPIC]
else
    echo "❌ No certificates were renewed."
fi
```

Cronjob
```crontab -e``` 
```30 3 * * * /home/rhoppe/scripts/certbot-renew.sh >> /var/log/certbot-renew.log 2>&1```

Don't forget to run:
`chmod +x /home/rhoppe/scripts/certbot-renew.sh`

To manually run:
`/home/rhoppe/scripts/certbot-renew.sh`
