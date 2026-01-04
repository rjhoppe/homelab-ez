---
layout: default
title: authelia
---

# authelia

[Official Documentation](https://www.authelia.com/)

Authelia is an open-source authentication and authorization server and portal fulfilling the identity and access management (IAM) role of information security in providing multi-factor authentication and single sign-on (SSO) for your applications via a web portal.

The way I have authelia setup is to force a redirect to authelia for authentication when a user tries to visit a particular subdomain of mine.

## Docker Compose Configuration

This is the configuration for the `authelia` service.

```yaml
services:
  authelia:
    image: authelia/authelia:4.39.9
    container_name: authelia
    restart: unless-stopped
    volumes:
      # Bind mounts for configuration files you manage on the host
      - ./authelia/configuration.yml:/config/configuration.yml
      - ./authelia/users.yml:/config/users.yml
    environment:
      - TZ=America/New_York
    # depends_on is only relevant for my current config
    depends_on:
      - lychee
    ports:
      - "9091:9091"
```

Example for configuration.yml

```yaml
---
theme: dark 
server:
  address: 'tcp://0.0.0.0:9091'

session:
  name: authelia_session
  secret: <YOUR-SECRET>
  domain: <YOUR-DOMAIN>
  same_site: lax
  expiration: 24h # 1 hour
  inactivity: 4h # 5 minutes
  remember_me: 1M

authentication_backend:
  file:
    path: /config/users.yml

access_control:
  default_policy: deny
  rules:
    - domain: <YOUR-DOMAIN>
      policy: one_factor
      subject:
        - "user:<YOUR-USER>"
        - "group:<YOUR-GROUP>"

storage:
  encryption_key: <YOUR-KEY>
  local:
   path: /config/db.sqlite3

notifier:
  filesystem:
    filename: /config/notifier.log

identity_validation:
  reset_password:
    jwt_secret: '<YOUR-JWT-SECRET>' 

log:
  level: info
```

Example users.yml

```yaml
users:
  family:
    displayname: <YOUR-DISPLAY-NAME>
    email: <YOUR-EMAIL>
    password: "<YOUR-ARGON2-HASH>"
    groups:
      - <YOUR-GROUP>
```
