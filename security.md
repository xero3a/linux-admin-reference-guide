# Security

This document outlines the system security measures implemented during the Linux administration setup. It includes configurations for user access, system hardening, firewall management, and service-level protections to ensure secure operation.

---

## SELinux Status

- SELinux is enabled and set to **enforcing** mode.
- Configuration checked with:
  ```bash
  sestatus
- Config file verified:
  a. /etc/selinux/config
# User & Access management 
- Root SSH login disabled
  ~ sudo nano /etc/ssh/sshd_config
  ~ #PermitRootLogin no
- Password Authentication disabled for SSH
- Public Key Authentication Permitted
- SSH Key pair generated
  ~ ssh-keygen -t ed25519
- SSH public key added to GitHub via web UI

# Firewall Configuration

Configured `firewalld` as the primary firewall management tool.

# Installation & Enablement
```bash
  ~ sudo dnf install firewalld -y
  ~ sudo systemctl enable firewalld
  ~ sudo systemctl start firewalld
# Basic usage
- Checking Status
  ~ sudo firewall-cmd --state
- Viewing default zones
  ~ sudo firewall-cmd --get-default-zone
- List active zones and rules
  ~ sudo firewall-cmd --list-all-zones

# Common Tasks
- Add a service (permanent)
  ~ sudo firewall-cmd --zone=public --add-service=ssh --permanent
- Applying changes
  ~ firewall-cmd --reload
- Opening ports
  ~ sudo firewall-cmd --zone=public --add-port=8080
