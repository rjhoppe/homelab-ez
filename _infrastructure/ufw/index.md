---
layout: default
title: ufw
---

# ufw

UFW (Uncomplicated Firewall) is the default firewall configuration tool for Ubuntu. It provides a user-friendly interface for managing `iptables` rules, making it easy to secure a server. It's particularly well-suited for host-based firewalls.

For more information, you can refer to the [official documentation](https://help.ubuntu.com/community/UFW).

## Helpful ufw CMDs and Info

Example of adding an allow rule (NOTE: the inclusion of the protocol at the end for your port)
```
sudo ufw allow from 127.0.0.1 to any port 80 proto tcp
```

Delete a specific rule
```
sudo ufw delete deny 80/tcp
```

Show all ufw rules and assign a numeric value to each rule
```
sudo ufw status numbered
```

Delete a rule by number (from the previous command output)
```
sudo ufw delete 4
```

Reload ufw after adding/deleting any rules
```
sudo ufw reload
```

Allow from any to a specific port (for testing only)
EX:
```
sudo ufw allow from any to any port 80 proto tcp
```
