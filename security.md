# Security

This document outlines the system security measures implemented during the Linux administration setup. It includes configurations for user access, system hardening, firewall management, and service-level protections to ensure secure operation.



   
### 1. SELinux Status

- Set SELinux to **enabled** and **enforcing** mode.
```
$ getenforce
# You’ll see one of:
#   Enforcing    → Already enabled
#   Permissive   → Enabled but not enforcing
#   Disabled     → Not active at all
```




- Check current configuration
```
$ sestatus
```

- Configuration checked with:
```
$ sestatus
```

- Edit the SELinux config file
```
$ sudo nano /etc/selinux/config
```

- Modify/Uncomment line:
```
~ SELINUX=enforcing
```

> Final Result:

```
SELINUX=enforcing
SELINUXTYPE=targeted
```

- Save, Exit, & Reboot

- Config file verified:
		```/etc/selinux/config```



### 2. User & Access management 

- Root SSH login disabled
```
$ sudo nano /etc/ssh/sshd_config

~ PermitRootLogin no
```

- Password Authentication disabled in SSH config to enfore key-based login.

- Public Key Authentication Permitted

- SSH Key pair generated
```
$ ssh-keygen -t <key_type>
```

- SSH public key added to GitHub via web UI



### 3. Firewall Configuration

> Previously configured `firewalld` as the primary firewall management tool.

#### Installation & Enablement

```
$ sudo dnf install firewalld 
$ sudo systemctl enable firewalld
$ sudo systemctl start firewalld
```


#### Basic usage

- Checking Status
```
$ sudo firewall-cmd --state
$ sudo systemctl status firewalld
```

- Viewing default zones
```
$ sudo firewall-cmd --get-default-zone
```

- List active zones and rules
```
$ sudo firewall-cmd --list-all-zones
$ sudo firewall-cmd --get-zones
$ sudo firewall-cmd --get-active-zones
```


#### Common Tasks

- Add a service (permanent)
```
$ sudo firewall-cmd --zone=public --add-service=ssh --permanent
```

- Applying changes
```
$ firewall-cmd --reload
```

- Opening ports
```
$ sudo firewall-cmd --zone=public --add-port=8080/tcp
```

- Allowing necessary services
```
$ sudo firewall-cmd --permanent --add-service=ssh
$ sudo firewall-cmd --permanent --add-service=http
```

- Removing unnecesssary services
```
$ sudo firewall-cmd --permanent --remove-service=dhcpv6-client
$ sudo firewall-cmd --reload
$ sudo firewall-cmd --list-all
```



### 4. User and Group Management

- Created non-root user accounts using:
```
$ sudo adduser <username>
```

- Set passwords for users
```
$ sudo passwd <username>
```

- Added users/groups for permission management
```
$ sudo usermod -aG <group> <username>
```

- Verify user group assignments
```
$ groups <username>
```

- Manage privileges by editing sudoers file:
```
$ sudo visudo
```

#### User added to wheel group

- Lock and disable user accounts when necessary
```
$ sudo passwd -l root		   # Locks root account for security purposes
$ sudo usermod -L <username>   # Locks accounts
$ sudo usermod -U <username>   # Unlocks accounts
```



### 5. SSH Hardening

- Disabled root login over SSH to prevent direct root access:
```
$ sudo nano /etc/ssh/sshd_config

~ Set: PermitRootLogin no
```

- Disable password authentication to enfore key-based login
```
$ sudo nano /etc/ssh/sshd_config
Set: PasswordAuthentication no
```

- Ensure only public key authentication is allowed


- Reload SSH target service
```
$ sudo systemctl reload sshd
```

- Created SSH key pair using secure alghorithm
```
$ ssh-keygen -t <key_type> -C "user.email@example.com"
```

- Copied public key to authorized keys
```
$ ssh-copy-id user@hostname
```



### 6. SELinux Advanced Tips

- SELinux enforces Mandatory Access Control (MAC) policies, restricting what processes can do.
```
- Common modes:
  # **Enforcing:** SELinux policy is enforced.
  # **Permissive:** Logs actions that would be denied but does not block them.
  # **Disabled:** SELinux is off.
```

- Check current mode:
```
$ sestatus
```

- Temporarily set permissive mode
```
$ sudo setenforce 0
```

- Permanently change mode, edit /etc/selinux/config
```
SELINUX=enforcing
```

- Use audit2allow to analyze denied actions and generate policy modules
```
$ sudo ausearch -m AVC,USER_AVC -ts recent
$ sudo audit2allow -M mypol
$ semodule -i mypol.pp
```

- Troubleshooting
  a. check /var/log/audit/audit.log
  b. check /var/log/messages

#### Custom SELinux Policy Module

> In some cases, default SELinux policies do not permit required operations. A custom policy can be written and installed to allow specific actions.

- Created a policy source file:
```
$ nano mypol.te
- Contents of `mypol.te`:
    ~ module mypol 1.0;

    ~ require {
    ~    type ssh_port_t;
    ~    class tcp_socket name_bind;
    ~ }

    ~ allow ssh_port_t self:tcp_socket name_bind;
```

- Compiled the module:
```
$ checkmodule -M -m -o mypol.mod mypol.te
```

- Created the policy package:
```
$ semodule_package -o mypol.pp -m mypol.mod
```

- Installed the policy:
```
$ semodule -i mypol.pp
```

> This approach can be expanded to handle other denials by inspecting logs (e.g., using `audit2allow`)



### 7. Service Hardening

- Listed all enabled services:
```
$ sudo systemctl list-unit-files --state=enabled
```

- Disable unnecessary services
```
$ sudo systemctl disable <service>
$ sudo systemctl stop <service>
```

- Verify firewall rules to block unwanted service ports
- Regularly check active services to ensure only needed daemons are available



### 8. Log Auditing

- `auditd` provides detailed logging of system calls and security-relevant events.

- Install auditd if not already installed:
```
$ sudo dnf install audit
```

- Enable and starting auditd
```
$ sudo systemctl enable auditd
$ sudo systemctl start auditd
```

- Verify auditd status
```
$ sudo systemctl status auditd.service
```

- View audit logs
```
$ sudo ausearch -m avc
$ sudo ausearch -ts recent
```

- Manipulate audit rules dynamically
```
#  a. auditctl
#  b. /etc/audit/audit.rules
```



### 9. Intrusion Detection (AIDE)

##### AIDE (Advanced Intrusion Detection Environment) is a host-based intrusion detection system that creates a database of files and their attributes. It can detect unauthorized changes to the 
##### system by comparing the current state with the baseline.

#### Installation

```
$ sudo dnf install aide
```

#### Initialization

- Create Database
```
$ aide --init
```

- Moved to proper directory
```
$ mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz
```

#### Manual Checks
- Verifying baseline changes
```
$ aide --check
```

#### Database Updates
```
$ aide --update
$ mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz
```

#### Automate AIDE Checks
```
$ crontab -e
	~ 0 1 * * * /usr/sbin/aide --check
```


### 10. Port Scanning & Enumeration Defense

##### Attackers often begin with reconnaissance, using tools like `nmap` to discover open ports and services. Hardening the system against such scans is a key part of proactive security.

#### Reduce Open Ports

- Keep only necessary services running and listening:
```
$ ss -tuln  # View listening ports
$ systemctl stop <service>
$ systemctl disable <service>
```

#### Limiting firewalld Exposure

- Allow SSH only from a trusted IP
```
$ sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="<X.X.X.X>" service name="ssh" accept'
$ sudo firewall-cmd --reload
```

- TCP/IP Stack Hardening
```
$ nano /etc/sysctl.conf
```

#### Ignore ICMP (ping) requests
```
$ net.ipv4.icmp_echo_ignore_all = 1
```

#### Drop malformed packets and prevent IP spoofing

```
$ sudo nano /etc/sysctl.d/sysctl.conf

net.ipv4.icmp_echo_ignore_all = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.all.accept_source_route = 0

```

- Apply changes
```
$ sudo sysctl -p
```


### 11. Fail2Ban Intrusion Prevention

##### Fail2Ban is a log-parsing tool that monitors system logs for patterns of failed login attempts and blocks the offending IP addresses via firewall rules.

#### Installation
- Fail2Ban can be installed from the EPEL repository:
```
$ sudo dnf install fail2ban fail2ban-firewalld
```

- Enable and Start service
```
$ sudo systemctl enable --now fail2ban
```

#### Basic Configuration
- Default config file /etc/fail2ban/jail/local
```
[sshd]
  enabled = true
  port    = ssh
  filter  = sshd
  logpath = /var/log/secure
  maxretry = 5
  bantime = 3600
```

#### Firewalld Integration
- Verify service is active
```
$ nano	
	banaction = firewallcmd-rich-rules
	backend = systemd
```

- Restart Service
```
$ systemctl restart fail2ban
```

####  Monitoring
- Verifying status
```
$ fail2ban-client status sshd
```
