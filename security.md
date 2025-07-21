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



## Firewall Configuration

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
  ~ sudo firewall-cmd --zone=public --add-port=8080/tcp



## User and Group Management
- Created non-root user accounts using:
  ```bash
  ~ sudo adduser <username>
- Set password for users
  ~ sudo passwd <username>
- Added users/groups for permission management
  ~ sudo usermod -aG <group> <username>
- Verify user group assignments
  ~ groups <username>
- Managed privileges by editing sudoers file:
  ~ sudo visudo
  # User added to wheel group
- Locked and disabled user accounts when necessary
  ~ sudo usermod -L <username>
  ~ sudo usermod -U <username>



## SSH Hardening
- Disabled root login over SSH to prevent direct root access:
  ```bash
  ~ sudo nano /etc/ssh/sshd_config
  # Set: PermitRootLogin no
- Disable pasword authentication to enfore key-based login
  ~ sudo nano /etc/ssh/sshd_config
  # Set: PasswordAuthentication no
- Ensure only public key authentication is allowed
- Reload SSH target service
  ~ sudo systemctl reload sshd
- Created SSH key pair using secure alghorithm
  ~ ssh-keygen -t ed25519 -C "user.email@example.com"
- Copied public key to authorized keys
  ~ ssh-copy-id user@hostname
- Verified SSH login works without password



## SELinux Advanced Tips

- SELinux enforces Mandatory Access Control (MAC) policies, restricting what processes can do.
- Common modes:
  - **Enforcing:** SELinux policy is enforced.
  - **Permissive:** Logs actions that would be denied but does not block them.
  - **Disabled:** SELinux is off.

- Check current mode:
  ```bash
  ~ sestatus
- Temporarily set permissive mode
  ~ sudo setenforce 0
- Permanently change mode, edit /etc/selinux/config
  ~ SELINUX=enforcing
- Use audit2allow to analyze denied actions and generate policy modules
  ~ sudo ausearch -m AVC,USER_AVC -ts recent
  ~ sudo auditallow -M mypol
  ~ semodule -i mypol.pp
- Troubleshooting
  a. check /var/log/audit/audit.log
  b. check /var/log/messages



## Service Hardening
- Listed all enabled services:
  ```bash
  ~ sudo systemctl list-unit-files --state=enabled
- Disable unnecessary service
  ~ sudo systemctl disable <service>
  ~ sudo systemctl stop <service>
- Verified firewall rules to block unwanted service ports
- Regularly check active services to ensure only needed daemons are available



## Log Auditing
- `auditd` provides detailed logging of system calls and security-relevant events.

- Install auditd if not already installed:
  ```bash
  ~sudo dnf install audit -y
- Enable and starting auditd
  ~ sudo systemctl enable auditd
  ~ sudo systemctl start auditd
- Verify auditd status
  ~ sudo systemctl status auditd.service
- View audit logs
  ~ sudo ausearch -m avc
  ~ sudo ausearch -ts recent
- Manipulate audit rules dynamically
  a. auditctl
  b. /etc/audit/audit.rules


