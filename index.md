---
title: Home
layout: home
---

# homelab-ez

Welcome to my homelab documentation.

This site contains a collection of notes, guides, and configurations for my personal homelab setup.

You can browse the available documentation using the navigation menu. 

Or visit the repo [here](https://github.com/rjhoppe/homelab-ez)

## Security Considerations

Securing a homelab is crucial to protect your local network and any services you expose to the internet. Here are some fundamental security practices and tools to consider.

### The Principle of Least Privilege
When configuring your homelab, always follow the principle of least privilege. This means that any user, program, or process should have only the minimum privileges necessary to perform its function. For example, don't run services as `root` if they don't need to, and ensure file permissions are as restrictive as possible.

### UFW (Uncomplicated Firewall)
A firewall is your first line of defense. **UFW** is a user-friendly firewall for Linux that makes it easy to manage network traffic. 

**Why it's important:**
*   **Controls Access:** It allows you to define rules for incoming and outgoing traffic, ensuring that only trusted connections are allowed.
*   **Default Deny Policy:** A best practice is to set UFW to deny all incoming traffic by default and then specifically allow only the services you need (like SSH on a non-standard port, HTTP/HTTPS). This significantly reduces your server's attack surface.

You should always have a firewall enabled on your gateway/router and on the server itself.

### Fail2Ban
**Fail2Ban** is an intrusion prevention software that protects servers from brute-force attacks. It monitors log files (e.g., for SSH, Nginx) for suspicious activity and temporarily or permanently bans IP addresses that show malicious signs.

**Why it's important:**
*   **Automated Defense:** It automatically blocks IPs after a configured number of failed login attempts, stopping many automated attacks in their tracks.
*   **Protects Various Services:** Fail2Ban can be configured to protect a wide range of services, not just SSH.

### Nginx as a Reverse Proxy
Using **Nginx** as a reverse proxy adds a significant layer of security and flexibility to your homelab.

**Why it's important:**
*   **Hides Backend Servers:** Your internal services are not directly exposed to the internet. Nginx acts as an intermediary, forwarding requests to the appropriate service. This hides the identity and characteristics of your backend servers.
*   **SSL/TLS Termination:** Nginx can handle all SSL/TLS encryption and decryption, centralizing certificate management and simplifying the configuration of your backend services.
*   **Load Balancing:** If you run multiple instances of a service, Nginx can distribute the traffic, improving performance and reliability.
*   **Security Headers:** You can configure Nginx to add important security headers (like `Content-Security-Policy`, `Strict-Transport-Security`, etc.) to all responses, enhancing browser-level security.

### The Dangers of `docker.sock`
Some Docker containers require access to the Docker socket (`/var/run/docker.sock`) to function. While this is necessary for certain tools that manage other containers (like some reverse proxies or monitoring agents), it poses a significant security risk.

**Why it's dangerous:**
*   **Root-Level Access:** The Docker socket is a direct line to the Docker daemon, which runs as `root`. A container with access to the socket can effectively control the entire host system.
*   **Container Escape:** If a container with access to `docker.sock` is compromised, an attacker can use it to create new containers, delete existing ones, and even mount sensitive host directories (like `/etc` or `/`) into a new container, giving them full root access to the host.
*   **Read-Only is Not a Solution:** Even mounting the socket as read-only (`:ro`) does not mitigate the risk. An attacker can still use the read-only access to inspect other containers (potentially leaking secrets like passwords stored in environment variables) and can still create new containers with full read-write access to the host.

**Best Practice:** Avoid mounting `docker.sock` into containers whenever possible. If a service absolutely requires it, ensure you trust the image's source completely and understand the risks involved. Look for alternative solutions that don't require direct access to the Docker daemon. 