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

