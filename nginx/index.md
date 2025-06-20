---
layout: default
title: Nginx
---

# Nginx

## What is Nginx?

Nginx (pronounced "engine-x") is a powerful, high-performance web server, reverse proxy, load balancer, and HTTP cache. It's known for its stability, rich feature set, simple configuration, and low resource consumption.

## Why Use Nginx in a Homelab?

Nginx is one of the most useful tools you can have in a homelab. It acts as the "front door" for all of your self-hosted services, providing a single, clean entry point. Here are some of the key reasons to use it:

*   **Reverse Proxy:** This is the primary use case in a homelab. You can run many different web applications (like a dashboard, a media server, or a code server) on different ports on your server, and Nginx can route traffic to them based on the domain name (e.g., `dashboard.your.domain` or `media.your.domain`). This means you only need to expose one port (usually 443 for HTTPS) to the outside world, which is much more secure.
*   **SSL/TLS Termination:** Nginx can handle all the HTTPS complexity for your services. You can install your SSL certificates (e.g., from Certbot) on Nginx, and it will encrypt all traffic coming from the outside. The connection between Nginx and your internal services can be plain HTTP, which simplifies the configuration of your individual applications.
*   **Serving Static Content:** If you have simple websites or static content, Nginx is incredibly efficient at serving them directly.
*   **Load Balancing:** If you ever decide to run multiple instances of an application for redundancy or performance, Nginx can distribute the traffic between them.
*   **Easy Management:** It provides a centralized place to manage access control, logging, and routing for all of your web-based homelab services.

Below you will find a collection of useful commands and configurations for managing Nginx.

## Helpful Nginx CMDs and Info

Test new nginx config
```
sudo nginx -t
```

Reload new nginx config
```
sudo systemctl reload nginx
```

Create new available site
```
sudo nano /etc/nginx/sites-available/[your domain here]
```

Create symlink between new available site and sites enabled
```
sudo ln -s /etc/nginx/sites-available/portainer.rjhoppe.dev /etc/nginx/sites-enabled/
```

View logs
```
sudo cat /var/log/nginx/error.log
```

To enable websockets (specifically for ntfy), add this to the location section:
```
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "Upgrade";
```
