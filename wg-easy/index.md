---
layout: default
title: wg-easy
---

# wg-easy

wg-easy is a service that provides a simple web-based user interface to manage a WireGuard VPN server. It allows you to easily create, edit, delete, and manage WireGuard clients.

For more information, you can refer to the [official documentation](https://wg-easy.github.io/wg-easy/latest/).

# Config steps

Make sure docker-compose is installed 
```
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d'"' -f4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```sudo chmod +x /usr/local/bin/docker-compose```
```mkdir /docker/wg-easy```
```cd /docker/wg-easy```
```nano docker-compose.yaml```
Paste in docker-compose below, make sure ports are different on host and client
```
sudo docker-compose up -d
sudo docker-compose down
```

## Docker Compose Configuration

This is the configuration for the `wg-easy` service.

```yaml
services:
  wg-easy:
    image: weejewel/wg-easy
    container_name: wg-easy
    environment: 
      - WG_HOST={WG_HOST}
      - PASSWORD={PASSWORD}
      - WG_DEFAULT_DNS=1.1.1.1,1.0.0.1
    ports:
      - "51820:51820/udp"
      - "127.0.0.1:51821:51821/tcp"
    volumes:
      - ./wg-easy/config:/etc/wireguard
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.ip_forward=1
      - net.ipv4.conf.all.src_valid_mark=1
```
