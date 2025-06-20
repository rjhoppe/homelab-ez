---
layout: default
title: wg-easy
---

# wg-easy

wg-easy is a service that provides a simple web-based user interface to manage a WireGuard VPN server. It allows you to easily create, edit, delete, and manage WireGuard clients.

For more information, you can refer to the [official documentation](https://wg-easy.github.io/wg-easy/latest/).

### Installation Steps

1.  **Install `docker-compose`**

    This command downloads the latest version of `docker-compose` and makes it executable.

    ```sh
    sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d'"' -f4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    ```

2.  **Create Directories**

    Create the necessary directory for the `wg-easy` configuration.

    ```sh
    mkdir -p /docker/wg-easy
    cd /docker/wg-easy
    ```

3.  **Create `docker-compose.yaml`**

    Create a file named `docker-compose.yaml` in the `/docker/wg-easy` directory with the following content.

    > **Note:** You must replace `{WG_HOST}` with your server's public IP address or domain name, and `{PASSWORD}` with a strong password for the web UI.

    ```yaml
    ---
    services:
      wg-easy:
        image: weejewel/wg-easy
        container_name: wg-easy
        environment:
          # REQUIRED:
          # Set this to your server's public IP address or domain name.
          - WG_HOST={WG_HOST}

          # OPTIONAL:
          # Set a password for the web UI.
          - PASSWORD={PASSWORD}

          # OPTIONAL:
          # Default DNS servers for clients.
          - WG_DEFAULT_DNS=1.1.1.1,1.0.0.1
        ports:
          - "51820:51820/udp"
          - "127.0.0.1:51821:51821/tcp"
        volumes:
          - ./config:/etc/wireguard
        restart: unless-stopped
        cap_add:
          - NET_ADMIN
          - SYS_MODULE
        sysctls:
          - net.ipv4.ip_forward=1
          - net.ipv4.conf.all.src_valid_mark=1
    ```

4.  **Start the Service**

    Run the following command to start the `wg-easy` container in the background.

    ```sh
    docker-compose up -d
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
